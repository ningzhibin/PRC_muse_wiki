# Paper deep read: MassNet — a billion-scale AI-ready mass spectral corpus

**Source**: Guo lab (corresponding: Tiannan Guo, Westlake University; Siqi Sun, Fudan University / Shanghai AI Laboratory; Zhiqiang Gao, Shanghai AI Laboratory; co-first authors Jun A., Xiang Zhang, Xiaofan Zhang, Jiaqi Wei — the Guomics team that also maintains DIA-BERT), *Nature Methods*, published online 2026-10-14, DOI: [10.1038/s41592-026-03247-y](https://www.nature.com/articles/s41592-026-03247-y) (OA, CC BY-NC-ND 4.0).

**In one sentence**: 648 projects, 27,643 LC-MS/MS runs and 1.54 billion DDA tandem spectra harmonized into a unified tensor format (MSDT) and used to train the de novo sequencing model XuanjiNovo; billed as proteomics' "ImageNet", but the labels are search-engine pseudo-labels, coverage is DDA-only, and unidentified spectra are excluded.

## 1. Structure and headline claims: reconciling the numbers

- **~30 TB** = raw data (648 projects from PRIDE + iProX, **27,643** LC-MS/MS runs, **35 species**, 10+ high-resolution platforms, **DDA-only**);
- **1.54 billion** = total MS/MS spectra;
- **548 million PSMs** = run-level identifications at 1% FDR from FragPipe (448M) and Sage (351M) each, merged and deduplicated; **4.45 TB** is their MSDT storage volume; after global FDR: **501M** PSMs / 2.04M unique peptides / 122,056 proteins.
- **Key gap: only ~36% of spectra were retained as identified**; ~64% unidentified spectra were discarded — precisely the spectra where de novo sequencing is most valuable (the authors admit sacrificing coverage of complex/marginal peptides).

## 2. The MSDT tensor format

**MSDT** = an **Apache Parquet** columnar-storage schema: one spectrum–identification pair per row, with scan number, precursor m/z/charge, RT, aligned fragment **m/z–intensity arrays**, peptide/protein mappings, target-decoy labels, and search-engine scores, plus CSV metadata (instrument, fragmentation mode, collision energy, project ID, software versions, species).

**Why a "tensor" lowers the barrier**: traditional MGF/mzML are decoupled from .msp/.sptxt/.tsv identifications, schema-less, and text-line-oriented; MSDT uses schema-defined numeric arrays that natively support batching, parallelism, and PyTorch/TF integration. After benchmarking 7 formats, Parquet was chosen (compression–IO tradeoff; csv.gz compresses best at ~5.89× but reads slowly at ~7.88 s); run-level files are **~82%** smaller than source data. Pragmatic detail: Parquet favors storage/sequential scans over random training access, so training converts to **LMDB** (storage decoupled from training; Casanovo likewise migrated from HDF5 to Lance).

**QC**: two engines searched independently, run-level FDR, only the lowest-PEP PSM kept per spectrum, global peptide/protein FDR 1% (PGA / picked_group_fdr); the training set additionally requires precursor mass error <0.1 Da, with train/validation split by non-overlapping peptide sequences.

**Comparison with prior resources**: MassIVE-KB (human-centric, strong provenance) and ProteomeTools (synthetic human library) are faulted on three points — human bias, peak lists decoupled from identifications, not AI-ready; MassNet keeps the full per-run peak list per spectrum rather than consensus spectra.

## 3. Learning paradigm: curriculum learning and "training collapse" (mechanism deep dive)

**"Training collapse"** = on MassNet-scale heterogeneous data, **up to 80% of NAT model training runs fail to converge** (early loss plateau/oscillation/divergence).

**Cause (two factors multiply)**: ① NAT predicts all tokens independently in parallel, so the hypothesis space grows exponentially (autoregressive decoding shrinks it token by token); ② CTC maps each label to **O(nT) alignment paths** (blank/repeat folding), far more than cross-entropy's single path. The paper formalizes **SPACE(NAT) ~ γ × TYPE(Loss) × ρ_ratio**, with TYPE(CTC) >> TYPE(CE). Distribution noise from 35 species × 10+ platforms drowns the gradient signal early → optimization fails.

**Solution (CTC path sampling, three steps)**: ① forward pass on empty input, enumerate valid paths folding to ground truth A, pick the **highest-probability** path ŷ (masking must happen on paths, not labels, since CTC paths have no one-to-one mapping to labels); ② randomly mask ŷ at ratio ρ to get ρ(A,ŷ) as decoder conditioning input; ③ still train with the **original CTC objective** (maximize total probability over all valid paths) to prevent memorizing a single path. **Difficulty schedule**: ρ=0 fully revealed (trivial task) → ρ→1 fully masked (pure generation); implementation: peek = max(**0.93** − epoch×**0.01**, 0). **Inference**: start fully masked → argmax gives y⁰ → iterative refinement conditioned on yⁱ⁻¹ (default 3 rounds) → **PMC** (a 0-1-knapsack variant, dynamic programming for the max log-prob sequence within precursor mass tolerance).

**Intuition** (for readers on the Karpathy track): downgrade "generate from scratch" to "BERT-style fill-in-the-blank" — early on, 93% of the answer is revealed (including the most likely alignment structure, implicitly guiding attention), only a few blanks are filled, then the hints are gradually withdrawn. The subtle part is **dynamically bootstrapped supervision** (each step picks the currently most likely path), forming an EM-like loop.

## 4. The XuanjiNovo model

**Architecture**: spectrum encoder (sinusoidal m/z encoding + intensity embedding → transformer) → NAT decoder (bidirectional self-attention + cross-attention to spectra, independent per-position softmax) → PMC mass constraint. Built on the π-PrimeNovo NAT+CTC framework.

**Training**: **100 million PSMs** (FragPipe-sourced only), MSDT→LMDB.

**Benchmarks** (in-house 15-species set: 307 raw files, 30.78M spectra, 9.66M PSMs, including Orbitrap Astral; baselines π-HelixNovo / Casanovo V2 / InstaNovo+ / ContraNovo / π-PrimeNovo): peptide recall human **0.45** (π-PrimeNovo 0.32 / InstaNovo+ 0.26), mouse **0.68** (0.60/0.52); K/Q near-isobaric discrimination **0.78/0.71** (baselines 0.71/0.68); scaling 30M→65M→100M: amino-acid recall 0.75→0.78→**0.82**, peptide recall 0.54→**0.68** (π-PrimeNovo ~0.30). **But**: on the classic nine-species benchmark the 100M version underperforms existing methods (distribution gaps: QE vs QE HF+, PEAKS 8.0 vs FragPipe 21.1); fine-tuning on 30M MassIVE-KB spectra yields **XuanjiNovo-130M**, which takes the lead (yeast +64.62%, human 0.60 slightly below ContraNovo 0.62). HCC set: 0.68 vs 0.38 (+77%).

**What "SOTA" precisely means**: leads on all species of their in-house 15-species benchmark; competitive across instruments/annotation pipelines only after fine-tuning — SOTA with conditions.

## 5. Data/code availability (verified)

- **PRIDE PXD071590**: paper states deposition (53,023 run-level MSDT files, global-FDR results, 100M training set, 15-species benchmark). ⚠️ Independent access verification failed in this read (project page fetch timed out); supporting evidence: MassNet-DDA issue #10 (2026-02-25) reported inaccessibility, closed 3-11. Zenodo backups: 10.5281/zenodo.17538551, 10.5281/zenodo.21703577.
- **Code**: `github.com/guomics-lab/MassNet-DDA` (XuanjiNovo code + weights `XuanjiNovo_100M_massnet.ckpt` / `_130M_massnet_massivekb.ckpt`, Google Drive/HF mirrors + SHA256; Docker `guomics2017/massnet-dda:cuda11_v1.1`/`cuda12_v1.1`).
- **MSDT-Converter**: `github.com/guomics-lab/MSDT-Converter` (mzML/Bruker .d/MGF/WIFF→MSDT and reverse; Docker `guomics2017/msdt-converter`).
- **"xuanjinovo-novotax"**: **unofficial**, a third-party fork (`mateuslab-prot/xuanjinovo-novotax`, adapted for the NovoTax pipeline).
- **License**: paper claims Apache 2.0; **verified** — the MassNet-DDA root LICENSE text is indeed Apache 2.0 (GitHub's "NOASSERTION" badge only means the classifier didn't recognize it). MSDT-Converter ships LICENSE.md and "MIT License.md" (not verified verbatim).

## 6. Critical assessment

- **DDA-only**: DIA/targeted explicitly listed as future work; DIA's multiplexed spectra are structurally different from DDA — no direct coverage for Astral DIA users.
- **Unidentified-spectra gap**: de novo's core value is resolving unknown spectra, yet ~64% unidentified spectra were discarded (authors admit sacrificing complex/marginal peptide coverage) — the biggest internal tension.
- **Species imbalance**: human/mouse dominated; human recall 0.45 far below yeast 0.69.
- **Annotation quality**: labels = search-engine pseudo-labels; scoring/FDR-strategy differences between the two engines introduce systematic bias; training used FragPipe-sourced PSMs only.
- **Narrow PTMs**: only Met oxidation, N-terminal acetylation (variable) + Cys carbamidomethylation (fixed); no phosphorylation/ubiquitination/glycosylation.
- **"ImageNet" is half true**: true — the scale/standardization/AI-readiness is a milestone; overstated — ImageNet has human ground truth, MassNet labels are noisy pseudo-labels, and only three tasks (de novo/RT/rescoring) are demonstrated.

## 7. Relevance for a core facility

- **(a) Running XuanjiNovo on own Astral DDA data**: feasible. Docker works out of the box (needs Linux + NVIDIA GPU, CuPy dependency, no macOS); input MGF (raw→mzML→MGF or MSDT-Converter); ~**280 spectra/GPU** (H200). Caveats: no Astral data and no rare modifications in training; human benchmark recall 0.45 — usable, but don't expect database-search accuracy.
- **(b) Relation to DIA-BERT**: different tasks (de novo generation vs DIA peak-group scoring), but shared ideas — transformer spectrum encoding, bidirectional context, large-scale training; and MassNet's rescoring demo (MassNet_4M) uses exactly the **DDA-BERT architecture** — the same team runs DDA/DIA tracks in parallel, so mastering NAT+CTC directly helps understand DIA-BERT's spectrum representations.
- **(c) Metaproteomics/database search**: MassNet databases were built per single-species Swiss-Prot, so direct transfer is limited; the MSDT format and curriculum-learning paradigm are reusable. Unfortunately, the scenario where de novo is most valuable for metaproteomics (unknown spectra) is exactly the discarded part; the third-party NovoTax fork shows someone is already trying to plug it into a taxonomy pipeline — worth tracking.

## Key takeaways

1. **Numbers**: 1.54B spectra (~30 TB) → 548M PSMs (4.45 TB) → 501M after global FDR; only ~36% of spectra retained — unidentified spectra are the biggest gap.
2. **The most transferable idea is CTC path-sampling curriculum learning**: "93%-revealed fill-in-the-blank, withdrawing 1% per epoch" progressively opens NAT's exponential alignment search space, turning 80% training failure into stable convergence — a general trick for NAT+CTC on noisy heterogeneous data.
3. **XuanjiNovo is SOTA on its in-house benchmark but needs fine-tuning across distributions**; Astral had zero training data yet performed well, but DIA is entirely out of scope.
4. **Code/weights Apache 2.0 (LICENSE verified), Docker out-of-the-box**; PRIDE PXD071590 access could not be independently verified in this read (there was a past outage issue).
5. **"ImageNet" half true**: scale and standardization are real, but pseudo-labels, DDA-only, no unidentified spectra — for DIA/metaproteomics users it's mostly a methodological reference.
