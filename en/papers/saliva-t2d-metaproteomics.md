# Paper deep read: salivary proteomics + metaproteomics finds molecular/taxonomic signatures of T2D

**Source**: Samodova D et al., *Microbiome* 2025, 13:5 (DOI registered 2024; DOI: [10.1186/s40168-024-01997-5](https://link.springernature.com/article/10.1186/s40168-024-01997-5); PMC: [PMC11720885](https://pmc.ncbi.nlm.nih.gov/articles/PMC11720885/)).
Authors: Diana Samodova, Evelina Stankevic, Mathilde Sonne Søndergaard, Naiyu Hu, Tarunveer S. Ahluwalia, Daniel R. Witte, Daniel Belstrøm, Anniek Frederike Lubberding, Pratik D. Jagtap, Torben Hansen*, Atul S. Deshmukh*.
Affiliations: Novo Nordisk Foundation Center for Basic Metabolic Research, University of Copenhagen (Hansen/Deshmukh labs) + Steno Diabetes Center Copenhagen + Aarhus University; Daniel Belstrøm is an oral-metaproteomics veteran (2016 PeerJ saliva metaproteomics in periodontitis), Pratik Jagtap comes from the Galaxy-P background.

**In one sentence**: saliva from 15 newly diagnosed T2D patients + 15 matched controls, profiled by 21-min diaPASEF for human proteome and metaproteome simultaneously; strong host immune/lipid-metabolism signals, microbial shifts (*Neisseria*↓, *Corynebacterium*↑), plus in-vitro insulin-secretion assays on selected bacterial peptides.

## 1. Study design

- **Cohort**: 15 screen-detected T2D cases (WHO 1999 criteria: fasting + 2h OGTT) + 15 normoglycemic controls (HC), drawn from the Danish ADDITION-PRO cohort (2009–2011, n=2082 risk-stratified cohort), matched on age, sex, and BMI.
- **Biggest strength**: participants did not know their diabetes status until the examination — **treatment-naive, no glucose-lowering drug confounding**. The methodologically hardest point of the whole paper.
- **Sampling**: Steno Diabetes Center Copenhagen; no tooth brushing on collection day; paraffin-wax-stimulated saliva; snap-frozen at −80°C. **Samples stored ~11 years (collected 2009–2011 → analyzed 2022)**; the paper says "kept un-thawed", but the effect of 11-year storage on protein integrity in a protease-rich fluid like saliva is **never discussed** — a hidden caveat.
- Confounders: oral hygiene, diet, physical activity not recorded; lipid-lowering/anti-inflammatory/antihypertensive medication not adjusted.

## 2. Experimental and analytical methods (details)

**Sample prep**: 20 µL saliva + 10 µL lysis buffer (1% SDC / 100 mM Tris pH 8.5 / 10 mM TCEP / 40 mM CAA), boiled 10 min at 99°C, sonicated 4 min; Lys-C 1:250 for 1 h → trypsin 1:100 overnight; quenched 1:10 with 10% TFA; 17,000×g spin; StageTip C18 cleanup; ~200 ng loaded on Evotips. **No abundant-protein depletion**.

**LC-MS**: Evosep One 60 SPD (21-min gradient), 8-cm PepSep column (150 µm ID, 1.5 µm C18), CaptiveSpray into timsTOF Pro 2, **DIA-PASEF**: m/z 400–1200, mobility 0.6–1.43, ramp/accumulation 100 ms, 32 windows of 25 Da (2×16-scan repeats), cycle time ~1.8 s.

**Search strategy (the trick worth learning)**: Spectronaut v17 directDIA + **16S-guided sample-specific bacterial database**:
1. Merge and dereplicate 16S V4 ASVs across samples;
2. BLASTn against HOMD 16S RefSeq, keep top 200 hits with score > 450;
3. Take all protein sequences for the corresponding HMT-ids → ~1,068,210 bacterial sequences + human Swiss-Prot (UP000005640, 2021-06-25).
4. 1% FDR (precursor + protein), **proteotypic peptides only** for quantification.

In-silico library: 7787 proteins (2212 protein groups) / 15,790 peptides / 24,385 precursors.

**Depth**: 4820 proteins total (1985 human + 2835 bacterial), on average 2231 per individual; 7452 bacterial peptides; 33 genera, 7 phyla. Dynamic range: 5 orders of magnitude (human), 4 (bacterial). High-abundance: KRT13/KRT4/KRT6A, ANXA1, IGKC/IGHA1, bacterial ribosomal proteins; low-abundance: UBR4, ADPRS, NDUFB10, etc. Median biological-replicate correlation r=0.91.

**Statistics**: protein level: log2 → keep ≥70% valid values → downshifted-normal imputation (1.8 SD downshift, width 0.3) → t-test, BH FDR<0.05; 1D enrichment FDR<0.02. Peptide level imputed with **missForest**. PCA: PC1+PC2 explain only 29.6%; group separation "moderate".

**Taxonomic assignment**: Unipept Desktop v2.0 LCA; 47% of bacterial peptides assigned to genus, 69% to phylum. Relative abundance = summed peptide intensity (or 16S reads) per genus / total.

## 3. Main results

### 3.1 Host proteins: 148 differential proteins (131↑ / 17↓, T2D vs HC)

Upregulated (human):
- **Immunity/inflammation**: multiple immunoglobulin variable regions (IGHV3-64, IGHV1-69, IGHV3-43, IGLV3-19, IGHG3, IGKV4-1, IGLV1-51, IGLV2-8) — **~40% are κ/λ light chains**, which the authors themselves flag as linked to autoimmunity/cancer; complement C3, C5, CFH, CFB; acute-phase proteins APCS, A2M, CP.
- **Lipid metabolism**: APOE, APOB, APOA4; proteasome subunits PSMA2/3/4/6, PSMB1, PSME2.
- **Positive-control-type hits**: RBP4 (insulin-resistance adipokine) and MUC7 upregulated — consistent with known T2D biology, suggesting the pipeline is on track.

Downregulated (human): keratins KRT4/KRT13/KRT78, keratinocyte sciellin (SCEL), RPS16, cytoskeletal SH3GLB2/SPTAN1/ANXA2, enzymes CILK1/PPP2R1A/NAGK.

1D enrichment: immune regulation, defense response, metabolism, translation/biogenesis, etc.

### 3.2 Bacterial proteins: very few changes (authors acknowledge the contrast with host proteins)

- Up: *Eubacterium* SEQF2744_00982, *Streptococcus* SEQF1066_02110 (BLASTp 100% match to **amylase-binding protein AbpA**, involved in dietary starch catabolism), *Parvimonas* membrane lipoprotein TmpC.
- Down: *Neisseria* Efem/EfeO-family lipoprotein and copper-containing nitrite reductase (BLASTp 100% *Neisseria* matches).
- **Honest but noteworthy**: BLASTp verification **disagreed** with Unipept on species assignment (database differences); the authors disclose it — peptide→species assignment is intrinsically unstable.

### 3.3 Taxonomic signatures (metaproteomics vs 16S)

- Metaproteomics top-5 genera: *Prevotella, Veillonella, Actinomyces, Rothia, Fusobacterium*; 16S top-5: *Veillonella, Prevotella, Haemophilus, Streptococcus, Rothia*. Top-20 relative abundances correlate at r=0.72 (p<0.01).
- 16S differential genera: *Rothia*↑ (p=0.05), *Schaalia*↑ (p=0.01), *Neisseria*↓ (p=0.05).
- Metaproteomics differential genera: *Haemophilus*↑ (p=0.02), *Neisseria*↓ (p=0.05).
- **Total-intensity t-test: only *Corynebacterium* significantly upregulated in metaproteomics**, detected exclusively in the T2D group (*Treponema* only in HC); no significant *Corynebacterium* difference in 16S.
- **The most robust taxonomic signal is decreased *Neisseria*** (consistent across both omics layers and protein level); the *Corynebacterium* increase is metaproteomics-only, unsupported by 16S.

### 3.4 Functions: oxidative stress + glycolysis

- 2D enrichment: peroxidase/AhpD-like and carboxymuconolactone decarboxylase-like enriched in T2D — but **each term rests on only 2–3 peptides** (p<0.005), statistically fragile.
- Peptide clustering of *Neisseria* + *Corynebacterium*: the *Corynebacterium* cluster enriched in glycolysis (GAPDH type I, NAD(P) binding).

### 3.5 Clinical associations + in-vitro validation (the paper's climax — and the part needing the biggest discount)

- Linear regression (adjusted for age, sex, smoking, glycemic status): multiple *Corynebacterium* and *Neisseria* peptides **negatively associated with 30-min OGTT plasma insulin** (FDR<0.05); two *Haemophilus* peptides negatively associated with waist circumference, one with fat %. No significant interaction with glycemic status.
- **Synthetic-peptide validation**: 14 top peptides synthesized by JPT → spiked into yeast tryptic background at gradients (1:1–1:20) → hybrid library built (Spectronaut 18 Pulsar) → raw data re-searched: **7/14 verified**, with calibration curves for saliva concentrations.
- **INS-1 GSIS**: 9 peptides at 1 µM each on rat insulinoma cells; screening showed several *Neisseria* peptides "nominally" increased insulin secretion (multiple t-tests, **uncorrected**); *Neisseria* peptide **ALLENLQEGSVIK** significantly increased secretion ~25% at high glucose (20 mM) (two-way ANOVA + Sidak). The authors' own benchmark: GLP-1 gives a 2–3-fold effect in INS-1; they concede **the mechanism by which salivary peptides would reach pancreatic β-cells is unknown**, and combined effects were not tested.

## 4. Critique (ranked by importance)

1. **The "precancerous state" reading is likely smoking-confounded.** Keratin downregulation → oral squamous-cell carcinoma/precancerous lesions is one of the flashiest claims, yet Table 1 shows more current smokers in T2D and more former smokers in HC — smoking is itself the strongest risk factor for oral precancerous lesions. The authors admit this in the Discussion, but "possible precancerous changes" survives in the Abstract/Conclusions. Without smoking as a covariate (n=30 may not support it), downgrade this to "smoking-driven effect cannot be excluded".
2. **The 50% synthetic-peptide validation failure rate (7/14) cuts both ways.** Framed as "targeted validation", but the flip side is: half the candidate peptides could not be confirmed in the hybrid-library re-search — directDIA peptide-ID FDR control on a million-sequence database is not as solid as implied. In metaproteomics, "identified" ≠ "really there" — worth remembering for methods people.
3. **Microbial effect sizes are weak overall.** Only 5 of 148 differential proteins are bacterial; PCA separation is moderate; *Corynebacterium* significance appears only in metaproteomics, not 16S, and "detected only in T2D" at n=15 is likely a detection-limit artifact rather than true absence. Only the *Neisseria* decrease (triple-consistent) is trustworthy.
4. **Functional enrichment on 2–3 peptides.** The peroxidase/AhpD terms each rest on 2–3 peptides — such enrichment can vanish with a different database version or filter threshold; not a mechanistic argument.
5. **The peptide→insulin-secretion causal chain is missing.** In-vitro 1 µM single peptides, rat INS-1 cell line, uncorrected screening p-values; between "this peptide is in saliva" and "it regulates human insulin secretion" lie absorption, circulation, and orders-of-magnitude concentration gaps. The exendin (Gila monster saliva) analogy is rhetoric, not mechanistic evidence. The authors acknowledge limits, but the headline narrative ("microbial applications for T2D management") runs ahead of the evidence.
6. **Imputation may inflate group differences.** Downshifted-normal imputation at protein level + missForest at peptide level; for systematically one-sided missingness (e.g., *Corynebacterium* only in T2D), post-imputation t-tests manufacture significance. The 70% presence filter is also lenient at n=15/group.
7. **11-year storage undiscussed.** Collected 2009–2011, analyzed 2022; saliva is protease-rich, and degradation/modifications (deamidation etc.) effects on quantification get no QC discussion.
8. BLASTp vs Unipept species-assignment disagreement — peptide→species assignment is unstable; the paper wisely retreats to genus level; treat any "species-level biomarker" claim with suspicion.

## 5. Strengths and transferable ideas

- **Treatment-naive screen-detected cohort**: no drug confounding — rare and valuable in clinical metaproteomics.
- **16S-guided database reduction**: 16S ASV → BLASTn → HOMD protein extraction shrinks the database from "all oral microbes" to taxa actually present, then directDIA. A **practical database-construction paradigm for clinical metaproteomics**; MetaLab-style pipelines could borrow the "marker-gene-first reduction" idea.
- **Synthetic-peptide hybrid-library re-search validation**: the gold standard for metaproteomic peptide-ID validation (JPT synthesis + yeast background gradient + Pulsar library); the 50% failure is itself informative, and the workflow is worth referencing for future clinical peptide validation in a core facility.
- **4800+ proteins in 21 min**: diaPASEF + Evosep as a throughput demo for clinical cohorts; saliva reaches this depth without depletion (though keratin KRT4/13/6A high-abundance contamination is intrinsic to saliva).
- Host protein signals (RBP4, complement, Ig light chains, APOE/APOB/APOA4) are consistent with known T2D biology — usable as positive controls for the analysis pipeline.
- Team composition (oral microbiology Belstrøm + T2D genetics Hansen + proteomics Deshmukh + Galaxy-P Jagtap) is the standard "clinical question × omics technology" combo.

## 6. One-sentence verdict

A **methodologically solid but narratively optimistic** clinical-metaproteomics resource paper: the host proteome is highly credible, the *Neisseria* decrease is the most robust microbial signal, while the two climax claims — "bacterial peptides regulate insulin secretion" and "precancerous state" — both carry clear confounding or evidence gaps; read with a discount. The biggest methodological takeaways are the **16S-informed database + synthetic-peptide hybrid-library validation** combo, and the cautionary 50% validation failure rate of directDIA on large databases.
