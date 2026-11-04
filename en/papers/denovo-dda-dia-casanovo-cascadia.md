# Deep Read: DDA / DIA de novo sequencing status — the Casanovo family and Cascadia

**Source**: On 2026-10-22, a chain of follow-up questions triggered by a CasanovoGUI news item, which also clarified the two DIA de novo lines (the third-party Casanovo-DIA vs. the Noble lab's own Cascadia). The literature involved is listed at the end.

**In one sentence**: For DDA de novo, the strongest evidence currently belongs to Casanovo; for DIA de novo, try Cascadia (the only one natively evaluated on the Astral 4 Th narrow window); both are only candidate generation and cannot stand alone as identification evidence.

## 1. DDA side: Casanovo itself + CasanovoGUI

**The GUI paper itself has no new benchmark** — it is a software description: it turns `casanovo sequence / db-search / train` into Java desktop tabs, automatically installs a private Python environment on first launch (matched to the GPU build), and results are viewed as annotated spectra (PDV), per-residue confidence, and peptide mapping back to a reference proteome (mismatch-tolerant mapping, but the GUI documentation gives no mismatch cap and no mapping-specific validation). When version / weights / config are aligned, GUI and CLI results should agree; **the GUI gives no accuracy bonus**.

Performance numbers can only come from the model papers, and note that the metrics are mostly precision–coverage / AP — **AP 0.95 ≠ 95% of peptides fully correct**:

- Nine-species benchmark (ICML 2022, leave-one-species-out): after precursor filtering, at coverage 0.53–0.68, peptide precision **0.67–0.82** and AA precision 0.90–0.95 (DeepNovo peptide precision only 0.25–0.46); forcing coverage = 1 drops peptide precision to 0.37–0.56 — part of the high scores comes from dropping low-confidence spectra
- *Nat Commun* 2024: nine-species training AP 0.83; switching to MassIVE-KB training with about 30 million PSMs → peptide AP **0.95** and AA AP 0.98; for ≥4+ charge, AP drops only about 12% (competitors >60%)
- Independent antibody evaluation (2023, using the old v3.0.0): best recall and AA precision among those evaluated; +ALPS assembly light-chain coverage 97.7–99.5%, heavy chain only 76.4–93.7% — strong at the peptide level, hard for full-length assembly
- Speed (official Performance page, Casanovo 5.2.1 + `casanovo_orbitrap_v5-2-0.ckpt`, Eclipse HeLa DDA with 86,311 spectra): GPU **530 s (163 spectra/s)** vs. CPU 10,926 s; it also completes with a VRAM limit of 3.5 GiB. **Note the version gap: the v5.2 checkpoint has no public accuracy benchmark; the AP numbers above are from earlier generations**

Honest limitations (general across this whole section):

- **Trypsin bias is measured**: the MassIVE-KB training set is all trypsin; on a non-enzymatic held-out set, the standard version has AP 0.60 and the specially fine-tuned Casanovo_ne 0.83. For non-trypsin scenarios, use the fine-tuned version or validate locally first
- **PTM vocabulary is limited** (2024 model: 28 tokens) — M oxidation, deamidation, N-terminal acetylation and others are present; **phosphorylation, glycosylation, and Lys acetylation / methylation / propionylation are not in the vocabulary and cannot be output**
- Long peptides (>18 AA) are clearly worse; I/L isobaric ambiguity is a physical limit; ETD/EAD have no systematic benchmark
- The score is a model probability-like score, **not an FDR/q value** — strict confidence control for de novo remains unsolved to this day

## 2. "Casanovo-DIA" — a third-party work of the same name, not made by Noble

- Ebrahimi & Guo (University of North Texas), a conference-paper-level research prototype from IEEE BIBE; it adds an encoder block to the Casanovo transformer to fuse MS1/MS2/precursor profiles
- On DeepNovo-DIA's 3 human DIA datasets, it beats DeepNovo-DIA and PepNet (AA-level precision +15–35%, peptide-level +59–81%)
- Code at github.com/Biocomputing-Research-Group/Casanovo-DIA (GPL); **it has not been merged into official Casanovo, and it is not in the GUI either**. When citing it, be careful not to treat it as a Noble lab product line

## 3. The DIA-side main entry: Cascadia (Noble lab, *Nat Methods* 2025)

**Core design — augmented spectrum**: In DIA, the evidence for one peptide is scattered across multiple spectra, and there is no quasi-precursor. Cascadia does not build DIA-Umpire pseudo-spectra, and it does not do MS1 feature detection: centered on one MS2 spectrum, it packs the 2 MS2 spectra before and after it in the same window (5 in total) + the isotope peaks from the corresponding 5 MS1 spectra, each peak carrying (m/z, intensity, time offset, MS1/MS2 label), and feeds them to a modified Casanovo transformer; at inference time it predicts every spectrum in the run, and the precursor is inferred by the model itself from the MS1 signal.

- Why this route is right: among peptides identified by DIA-NN, the proportion for which a DIA-Umpire MS1 feature can be found is 71.4% for the wide-window Lumos and **only 47.2% for the narrow-window Astral** — the pseudo-spectrum route inherently loses half on Astral data
- Training: the encoder is first pretrained on MassIVE-KB DDA data (**ablation: skipping pretraining and training directly on DIA data does not converge**), then a b/y ion classification auxiliary loss is added; the wide-window model is fine-tuned on 372 CPTAC runs with about 5M peptides; the narrow-window model is separately trained on 878,217 augmented spectra from 77 mouse plasma EV (Mag-Net) samples
- Results (peptide level, precision estimated by mapping back to the proteome):
  - Wide-window CPTAC (Lumos, 16 Th): at 90% precision, **2,371** peptides vs. DeepNovo-DIA 529; Casanovo on raw spectra 29, on pseudo-spectra 64
  - **Narrow-window Astral (4 Th)**: at 90% precision, 2,137 → **6,224** peptides; the score is well calibrated and can be used as a probability to set thresholds
  - Orthogonal validation: in skin samples, 159 candidate SNVs, 21 confirmed by exome sequencing (AUC 0.92), while DeepNovo-DIA had 57 candidates and 0 confirmed; in plasma EVs, 3,305 Ig candidate peptides, 47.7% supported by DIA-NN re-search (DeepNovo-DIA 32%)
- Shortcomings: **slow** — one Astral run takes about 5 h on an A100 (DIA-NN on the same data takes <30 min on CPU); GitHub has no formal release, and the PyPI documentation describes itself as WIP; trypsin bias remains (the authors suggest fine-tuning per application); training data for modified peptides are insufficient, and PTM prediction/localization were not evaluated; the timsTOF ion mobility dimension is unused; the evaluated precision is only a lower bound (failure to map back ≠ wrong)
- Input mzML, output mzTab / .ssl (can go into Skyline as a spectral library); Apache 2.0, github.com/Noble-Lab/cascadia

## 4. Conclusions for a core facility

- DDA de novo: Casanovo currently has the strongest evidence; its positioning = candidate generation for tryptic high-resolution DDA (unexplained spectra in metaproteomics, antibodies, samples with incomplete databases); it cannot stand alone as identification evidence — score threshold + mapping back + spectrum review are required, and key sequences need synthetic-peptide validation
- DIA de novo: try **Cascadia** (the only one natively evaluated on the Astral 4 Th narrow window); its positioning is as a DIA-NN complement, specifically digging out database-external novel peptides, directly relevant to metaproteomics / understudied-organism scenarios; the cost is mainly GPU time (about 5 h/run)
- Casanovo on Astral DDA has no dedicated benchmark (the official speed test used Eclipse), so the numbers cannot be directly extrapolated

**Literature involved**: Wen B et al. CasanovoGUI. *J Proteome Res* 2026 (DOI: 10.1021/acs.jproteome.6c00617); Yilmaz M et al. Casanovo model paper: ICML 2022; *Nat Commun* 2024 (DOI: 10.1038/s41467-024-49731-x); Ebrahimi S, Guo X. Casanovo-DIA. IEEE BIBE 2024 / arXiv:2402.11363 (PMC11044815); Sanders J, Wen B, et al. Cascadia. *Nat Methods* 2025, 22:1447–1453 (DOI: 10.1038/s41592-025-02718-y); supporting evidence: Beslic et al. independent evaluation of antibody de novo tools, *Brief Bioinform* 2023.
