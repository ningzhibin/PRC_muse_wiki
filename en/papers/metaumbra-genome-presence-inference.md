# Deep read: MetaUmbra — genome-level presence inference from metaproteomic peptides

**Source**: Wu Q, Ning Z, Zhang A, Cheng K, Figeys D. MetaUmbra: Statistically Controlled Genome-Level Presence Inference from Metaproteomic Peptides. bioRxiv 2026.04.29.721689**v2** (DOI: 10.64898/2026.04.29.721689). The v2 revision updated the statistical framework and benchmark analyses and added Monte Carlo sensitivity analyses and leave-one-reference-out experiments. Preprint, not peer-reviewed. Code: https://github.com/byemaxx/MetaUmbra (BSD-3-Clause).

**In one sentence**: MetaUmbra does not estimate abundance, differential expression, or function — it answers one question: given a user-defined genome panel and an identified peptide table, is each genome in the panel present in the sample, with a p-value and q-value? Its core splits peptide evidence into unique and shared components: shared peptides are kept, weighted by degeneracy and tested against a Monte Carlo empirical null, unique peptides are calibrated against background genomes, the two evidence streams are combined by a Simes intersection test plus closed testing, and **a genome with no unique peptide can never be called significant**.

## 1. The problem

- In metaproteomics many peptides come from conserved proteins and belong to several genomes, so peptide-to-genome mapping is inherently ambiguous. The gap in existing tools: Unipept uses the lowest-common-ancestor rule, pushing ambiguous evidence to higher taxonomic ranks — robust but low-resolution, and it does not use peptide scores in inference; protein-centric workflows such as MetaLab infer proteins first and then annotate taxa, adding protein-grouping assumptions and bias; Peptonizer2000 gives taxon-level Bayesian confidence scores and MiCId gives species/genus-level statistical confidence, but neither returns genome-level p-values and q-values for a **user-defined genome panel**.
- MetaUmbra fills that gap: peptide-centric, genome-level, formally tested, applicable to custom strain panels, MAG catalogues, combined host–microbe references, or iteratively refined follow-up panels. Every inference is relative to the composition of the panel.

## 2. Workflow overview: two steps, digest once

1. **Reference construction**: one protein FASTA file per genome, digested *in silico* with RPG v2.0.5 to produce genome-specific theoretical peptide reference tables. This only needs to be done once for a given genome collection and digestion setting.
2. **Genome presence scoring**: an observed peptide table (MaxQuant or DIA-NN) is matched against the references and evaluated in the statistical framework, producing per-genome p-values, BH-adjusted q-values, and presence scores.

Interfaces: GUI (`metaumbra-gui`), command line (`metaumbra digest` / `metaumbra score` / `metaumbra extract-parquet`, the last converting DIA-NN parquet reports to peptide TSVs before scoring), and a Python function-based workflow; integrated into the peptide-annotation workflow of MetaX v2.0 (Wu et al., *Anal Chem* 2025;97:9739–9747). Python 3.10+, `pip install metaumbra[all]`.

## 3. The theoretical peptide reference: map the ambiguity first

- **Digestion settings** (as used in the benchmarks; must match the upstream identification workflow): Trypsin/P, peptide length 7–30 aa, up to two missed cleavages. Retained peptides are exported per genome as reference tables, preserving the peptide → protein → genome link that degeneracy calculation needs.
- **I/L-equivalent matching**: because MS generally cannot distinguish isoleucine from leucine, the primary analysis maps both I and L to `J` before deduplication, matching, and degeneracy calculation; exact-sequence matching is kept as a sensitivity analysis. Q/K and AD/W are not collapsed.
- **How ambiguous is the reference space** (4,744 UHGG representative genomes, digested *in silico*): **384.1 million nonredundant theoretical peptides, 84.3% unique to a single genome and 15.7% shared by at least two genomes**. Sharing is concentrated at low degeneracy: 13.9% of all theoretical peptides fall in the 2–5 bin, 1.52% in 6–20, only 0.21% above 20, 0.036% in 101–500, and 0.004% shared by more than 500 genomes. Ambiguity is substantial but predominantly local, not global.
- **Theoretical peptide counts vary widely between genomes**: median 108,482 nonredundant theoretical peptides per genome, with a heavy right skew — so raw matched-peptide counts cannot be compared across genomes without calibration. This is why the unique-evidence model uses the theoretical unique-peptide opportunity (§4.3).
- When the SIHUMIx peptide set was matched against the full UHGG background, all 4,746 candidate genomes retained at least one match — matching counts alone are insufficient.

## 4. The scoring machinery (the core)

### 4.1 Preprocessing: scores are only relative weights

- Identifications are filtered using decoy and peptide-error fields (e.g. PEP); after I/L normalization, duplicates are collapsed keeping the highest evidence score; retained scores are min–max scaled to [0,1]; peptides with no informative score get s(p) = 1.
- **Key point**: these scores are used as relative evidence weights, not identification probabilities; peptide-error measures are kept separate from the score weights and from the fixed α = 0.05 used by the unique-evidence models.

### 4.2 Degeneracy weighting: shared peptides are kept, discounted by ambiguity

- For genome k, let τ_k be its theoretical peptide set and O the observed peptide set; the matched set is M_k = O ∩ τ_k.
- Each observed peptide p gets a **degeneracy d(p)** = the number of genomes in the panel containing p. Weight w(p) = 1/d(p): unique peptides (d = 1) keep full weight, shared peptides are discounted by degeneracy. Genome k's weighted evidence is E_k = Σ_{p ∈ M_k} w(p)·s(p).
- Shared evidence = matched peptides with d(p) > 1; unique peptides have d(p) = 1.

### 4.3 Shared evidence: a peptide-space Monte Carlo empirical null

- Weighted contributions w(p)·s(p) from observed shared peptides are pooled by degeneracy bin: **2–5, 6–20, 21–100, 101–500, >500** (bin edges were not optimized against benchmark outcomes).
- For each genome, a genome-specific null distribution is generated by sampling from these pools according to that genome's matched shared-peptide bin counts; the empirical p-value is p_S = (1 + #{Monte Carlo replicates with shared evidence ≥ observed}) / (1 + B), with B replicates.
- Replicates: 500 in the first stage (seed 1); near-threshold refinement uses 20,000 for the individual Mix24X, pooled near-genus, and hamster analyses, and 200,000 for SIHUMIx and pooled full-UHGG Mix24X.
- The point: test whether observed shared support exceeds what peptide ambiguity alone would produce, **without needing a separate decoy-genome database**.

### 4.4 Unique evidence: two null models, automatic selection

- First compute each genome's **theoretical unique-peptide opportunity A_k**: the number of distinct theoretical peptides assigned only to that genome within the active panel (after normalization, before scoring).
- **Empirical-background model** (when adequate comparable background exists): the observed unique-peptide count U_k is compared with weak-background genomes having similar A_k. The candidate itself and the highest-support candidates are excluded from the background; the statistic uses the 95th percentile Q_0.95,k of unique-peptide counts in the comparable background, with α = 0.05 and a numerical floor of 10⁻³⁰⁰. Background exclusion is iterative: initial exclusion fraction 0.10; each round excludes the highest-ranked fraction by unique-peptide count, recomputes the statistics, combines, and BH-adjusts; the next exclusion fraction is the proportion of matched genomes with intermediate q ≤ 0.20, bounded to [0.05, 0.20]; updating stops when the absolute change is < 0.01 or after five iterations. Intermediate q-values are used only for background estimation and do not define formal presence calls.
- **Alpha-upper-bound model** (when adequate comparable background is unavailable): a multiplicative model treating α as an upper bound on retained unique-peptide error contributions. **The paper states explicitly that it depends on peptide-dependence and error-bound assumptions and should not be interpreted as a posterior probability of genome presence.**
- **Automatic selection rule**: using theoretical-opportunity strata, requiring at least 100 comparable genomes, an expected upper-tail count of at least 5, and adequate background for at least 90% of candidates, the empirical-background model is used; otherwise the whole panel uses alpha-upper-bound. Selection never uses pilot p-values, pilot q-values, benchmark labels, recovery, or additional-call counts. The large SIHUMIx and full-UHGG Mix24X panels used empirical-background; the 24-genome individual Mix24X and 73-genome near-genus panels used alpha-upper-bound; the hamster analyses explicitly used alpha-upper-bound.
- The README agrees: the default unique-evidence mode is `auto`; it partitions candidates by theoretical peptide opportunity and checks whether at least 90% have at least 100 comparable background genomes and at least five expected observations above the configured background quantile; if structurally eligible it uses `empirical-background`, otherwise the entire panel uses `alpha-upper-bound`; the decision variables and reason are recorded in `artifacts/diagnostics/unit_empirical_background_calibration.tsv`.

### 4.5 Combination: Simes intersection test + closed testing, with unique evidence as the gate

- Unique-evidence p_U and shared-evidence p_S come from the same observed peptide set and reference panel, so independence cannot be guaranteed; Fisher's combination (which assumes independent component p-values) is **kept only as a sensitivity analysis**.
- Primary analysis: p_U and p_S are first evaluated jointly with a **two-component Simes intersection test**, then the final genome-level p-value is obtained by **closed testing of the unique-evidence component**. The formula from the README:

  `p_G = max(p_U, min(1, 2·min(p_U, p_S), max(p_U, p_S)))`

- **When U = 0 (no observed unique peptide), p_G = 1.** So p_G cannot be smaller than p_U, and **shared evidence alone cannot establish a formal genome-presence call**.
- Genome-level p-values are **BH-adjusted** separately within each independently analyzed peptide set and candidate panel, across genomes with at least one observed peptide match. Primary threshold q ≤ 0.01, secondary q ≤ 0.05. The presence score is a q-value-based ranking score used for visualization (the formula is given as a figure in the paper). Harmonic-mean, Bonferroni, and unique-only alternatives are retained for sensitivity comparison.

## 5. Benchmark results

### 5.1 SIHUMIx: 8/8 recovered, zero additional calls

- Panel of 4,746 genomes (8 expected + 4,738 UHGG background, after removing six species-overlapping background genomes). **All eight expected SIHUMIx members were recovered with no additional significant calls** among the background, at both q ≤ 0.01 and q ≤ 0.05. Raw p-values for expected members ranged from 2.0×10⁻³⁰⁰ to 3.13×10⁻⁸; BH-adjusted q-values from 3.164×10⁻²⁹⁷ to 1.854×10⁻⁵.
- The ranking was compact: the top eight genomes accounted for 99.8% of cumulative matched-peptide coverage. But the unique/shared structure varied enormously: *Lactobacillus plantarum* had only 6 unique / 189 shared matched peptides, *Clostridium butyricum* 25/441, while *Erysipelatoclostridium ramosum* had 2,986/3,360 and *Bacteroides thetaiotaomicron* 5,057/23,879. **Weak expected members (6 unique peptides) stayed significant, while background candidates supported only by shared peptides were blocked by the U = 0 rule** — the design of §4.5 working as intended.

### 5.2 Mix24X: 24/24 recovered; near neighbours are the hard part

- **Individual-strain analyses** (24 peptide sets, each against the exact 24-genome panel): every expected genome was recovered in all 24 analyses; 20 had no additional call, 4 had one additional call involving the *Bacillus cereus*/*B. thuringiensis* and *Salmonella bongori*/*Shigella flexneri* pairs.
- **Pooled analyses** (all 24 peptide sets merged): in both the near-genus panel (73 genomes) and the full-UHGG panel (4,764 genomes), all 24 expected genomes remained within ranks 1–24; the largest rank shift was *Bordetella parapertussis*, from rank 5 to 16. The near-genus panel produced 3 additional calls, the full-UHGG panel only 1 — **expanding the background did not increase additional calls**, but it redistributed peptides between unique and shared categories (median unique-peptide fraction across the 24 expected genomes fell from 61.41% to 56.43%; median change −83.5 unique assignments per genome) and changed the applicable unique-evidence model (alpha-upper-bound for near-genus, empirical-background for full UHGG). Difficult expected genomes kept most of their unique support: *B. cereus* retained 148 of 153 near-genus unique peptides, *B. thuringiensis* 47 of 49, *Shigella flexneri* 117 of 134.

### 5.3 Comparison with Unipept and Peptonizer2000 (descriptive, not an equivalent benchmark)

- The external tools' outputs were retained from the previous analysis and not rerun after the MetaUmbra scoring revision, so the comparison is descriptive — recovery and output compactness only.
- SIHUMIx: Unipept recovered all eight expected members plus five additional taxa; Peptonizer2000 reported 26 taxa overall, and at the recommended 0.8 confidence cutoff it no longer retained *C. butyricum* or *L. plantarum*; MetaUmbra retained all eight with no additional significant calls.
- Mix24X: Unipept recovered all 24 expected species with 39 taxa overall; Peptonizer2000 reported 36 taxa and recovered 23 of 24, losing *B. parapertussis*, *B. cereus*, *B. thuringiensis*, *P. putida*, *S. flexneri*, and *Cellulophaga lytica* at the 0.8 cutoff; MetaUmbra retained all 24 in both expanded panels, with 27 significant genomes (near-genus) and 25 (full UHGG).
- Note: taxon-level and genome-level outputs have different inferential targets and should not be treated as directly equivalent; "additional reports" are defined relative to benchmark membership, not as estimates of BH false-discovery rate.

### 5.4 Hamster gut MAG panel: evidence structure in real data

- 706 GTDB-Tk-annotated bacterial MAGs (926 bins from co-assembly of 90 shotgun metagenomes). No closed expected set, so this is **not a recovery benchmark** — it assesses evidence structure in a dense real-data reference space.
- I/L-equivalent matching with the alpha-upper-bound model: **587 of 706 MAGs were significant at both q ≤ 0.01 and q ≤ 0.05**; exact-sequence matching gave 588 significant MAGs, with 578 shared between the two analyses (Jaccard index = 0.9682), so the overall result was largely preserved while a small number of calls depended on I/L normalization.
- Strongly supported MAGs included *Escherichia coli*, *Desulfovibrio* sp., and MAGs related to Bacteroidaceae, Muribaculaceae, Lachnospiraceae, and Bifidobacteriaceae. Because several candidates reached the same minimum formal q-value, formal significance alone could not order them, so the authors defined a **descriptive display score** (sum of the standardized log-transformed unique-peptide count and the standardized shared-evidence significance) for visualization only.

### 5.5 Supplementary evaluations

- Two further published mixture datasets were evaluated as supplementary analyses (Supplementary Figures S3 and S4): a six-strain culture mixture (Valdés-Mas et al., *Cell* 2025; PXD057701) and the ATCC 20-strain SPEED standard (PXD011189).

## 6. Robustness: three numbers worth remembering

- **No significant call rested on 0 or 1 unique peptides**: using 1–3 unique peptides as a descriptive low-support category, such records accounted for 0/8 significant SIHUMIx candidates, 2/27 pooled Mix24X near-genus candidates, 0/25 pooled full-UHGG candidates, and 1/28 candidates across the 24 individual Mix24X analyses — all three low-support benchmark records were additional calls, each with exactly three unique peptides. In the hamster panel, 71/587 significant MAGs fell in this low-support category.
- **Monte Carlo settings did not matter**: across 27 combinations of three benchmark panels, three degeneracy-bin schemes, and three random seeds (1, 7, 42), the significant call sets were identical to the corresponding default analyses (Jaccard index = 1).
- **Leave-one-reference-out: an incomplete panel creates false calls.** Removing one expected reference kept all remaining expected genomes (7/7 SIHUMIx, 23/23 Mix24X in each omission scenario), but at q ≤ 0.01 it produced **73 newly significant SIHUMIx events and 59 Mix24X events**. The mechanism was consistent: total matched-peptide counts were unchanged, while peptides previously shared with the omitted reference became panel-unique to another candidate; across the 132 newly significant events, panel-unique counts increased and shared counts decreased correspondingly, and 117 candidates had no panel-unique peptide in the baseline analysis. Sequence-level mapping confirmed it: 9,512 SIHUMIx and 1,334 Mix24X peptide-event records gained panel uniqueness after omission, and each sequence had originally matched exactly the newly called candidate and the omitted reference.
- **Computational cost**: component scoring took 43.6 s and 2.35 GiB peak memory for the 73-genome near-genus panel, versus ~18 min and 47 GiB for the 4,764-genome full-UHGG and 4,746-genome SIHUMIx panels. Reference digestion was benchmarked separately on a laptop with 16 workers: 100, 200, and 500 UHGP genome FASTA files took 69.7, 139.5, and 345.9 s, with peak memory below 0.44 GiB. Sensitivity analyses used MetaUmbra 1.5.0.

## 7. Limitations and usage boundaries (as argued in the Discussion)

- **Inference is conditional on the panel**: panel-unique support should not be read as universally species-specific; significant genomes should be considered together with plausible close competitors and reference completeness. Discrimination among very closely related genomes remains limited by extensive peptide sharing, though iterative refinement from broad panels to densely sampled species- or strain-level references may improve resolution.
- **Do not use fixed unique-peptide count thresholds** (e.g. "more than three unique peptides"): a peptide that appears unique in a smaller or sparser database may no longer be unique after background expansion or inclusion of more closely related genomes; fixed thresholds are not a stable basis for presence inference across reference spaces. MetaUmbra's formal framework is meant to replace that practice.
- **Other limitations**: dependence on upstream peptide-identification quality, score calibration, and digestion assumptions; fragment-level dependence in DIA data is not explicitly modeled. MetaUmbra is intended as a genome-presence module within broader metaproteomics workflows and does not itself estimate abundance or perform differential or functional analysis.
- **Data**: SIHUMIx PXD023217 (DDA, MaxQuant 2.6.7.0); Mix24X PXD005728 (DDA, Mascot 2.5.1); hamster DIA-PASEF PXD057158 (DIA-NN 1.9.1); hamster MAG reference Zenodo 13909289. Funded by NSERC RGPIN-03905-2018.

## 8. Implications for small-panel applications

- The hardest part of the design to simplify is the null: a 1–3 genome panel has no comparable background, so it automatically falls to the **alpha-upper-bound** model (the precedent being the 24- and 73-genome panels), which is explicitly not a posterior probability of presence.
- The other hard rule that cannot be simplified away: **U = 0 gives p = 1**. Any simplified tool that drops it lets shared peptides alone "prove" presence, and the whole framework's conservatism is gone.
- The input is only a peptide table (DIA-NN or MaxQuant), independent of the search itself; but scoring quality depends entirely on the upstream identification and on digestion settings matching (Trypsin/P, 7–30 aa, ≤2 missed cleavages).

## 9. Output files

- `unit_genome_results.tsv`: one row per analysis unit × genome, with q-values and threshold flags; `cohort_genome_summary.tsv`: recurrence across analysis units; `sample_unit_mapping.tsv`: validated sample assignment; `genome_selection_manifest.json`: the only downstream genome-selection interface (q ≤ 0.05 genomes per unit, and the stricter q ≤ 0.01 subset); `artifacts/`: run metadata, logs, caches, and optional diagnostics. Key columns: `num_peptides_matched`, `num_peptides_unique`, `theoretical_unique_peptides`, `expected_unique_null`, `unique_depth_fold` (observed unique / null-expected), `has_unique_evidence`, `pvalue_shared`, `pvalue_unique`, `pvalue_simes_intersection`, `pvalue_simes_closed` (the default `pvalue`), `qvalue`, `presence_score`.
