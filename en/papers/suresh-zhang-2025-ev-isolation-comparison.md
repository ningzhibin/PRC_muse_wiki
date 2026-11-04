# Deep Read: Suresh & Zhang 2025 — Comprehensive Comparison of 9 Plasma EV Isolation Methods

**Source**: Patil Shivprasad Suresh, Qibin Zhang (corresponding author; UNC Greensboro, Center for Translational Biomedical Research / Dept. of Chemistry & Biochemistry), *J Proteome Res* 2025, 24(6):2956–2967 (DOI: 10.1021/acs.jproteome.5c00149; PMID: 40356199; PMC: PMC12150312; CC-BY 4.0). Data: PRIDE **PXD058948**; funding: NIDDK R01 DK114345.

**In one sentence**: The value of this paper is not that "MagNet won", but that it places MagNet's 785 proteins inside a unified workflow that uses **non-on-bead digestion, a 21-minute gradient, and library-free** searching — this is a reasonable reference for eluted samples, not the 4,000+ ceiling of Wu et al. 2025. What differs is not the beads, but the entire downstream workflow.

## 1. Study design: in one sentence

The same pooled plasma, 100 µL per method, triplicates, 9 methods (4 principles) compared after single-method isolation using the same downstream pipeline (NTA + Simple Western + bottom-up DIA proteomics) for yield, size distribution, purity, and protein coverage. The authors describe it as the first detailed characterization of 9 methods using 100 µL of plasma. Combination methods (DGUC+SEC, etc.) were intentionally excluded, on the grounds that clinical small-volume samples cannot afford multi-step losses.

- **Sample**: Healthy-donor EDTA plasma, from a clinical trial of functional-food intervention for exercise stress recovery (NCT03445234), collected as plasma after 2000*g* for 10 min, then pooled and stored at −80°C; before use, thawed at room temperature and centrifuged at 3000*g* for 10 min to remove debris. **Frozen pooled plasma** — this is the largest external-validity limitation of the whole paper (see §6).
- **Unified normalization**: EVs isolated by all methods were concentrated / buffer-exchanged through 10 kDa Amicon to 100 µL PBS, removing residual polymers, salts, and isolation reagents. This step makes the downstream comparison relatively fair, but it also means: this paper's MagNet result **is not equal to** Wu's full MagNet workflow result (see the end of §2).
- **The 9 methods**: centrifugation methods UC and DGUC; SEC methods qEVsingle 35 nm and SmartSEC; polymer precipitation methods ExoQuick ULTRA and Total Exosome Isolation (TEI); electrostatic / affinity methods MagNet (MagReSyn SAX), Norgen NPR (SiC resin), and MagCap (Tim4-PS affinity).

## 2. The nine methods in detail (only key parameters and MagNet's fork point)

- **UC**: 100 µL diluted to 11.9 mL PBS, 150,000*g* for 3 h, pellet resuspended and centrifuged again at 120,000*g* for 3 h. The longest time tier.
- **DGUC**: UC pellet layered onto a 5/10/20/40% iodixanol gradient, 120,000*g* for 18 h, upper 6 mL collected, then 120,000*g* for 4 h. Density separation: purity traded for yield.
- **qEV**: qEVsingle 35 nm, 0.7 mL void discarded, the next 0.7 mL collected.
- **SmartSEC**: 100 µL + 400 µL PBS loaded onto the column, rotated at room temperature for 30 min, eluted at 500*g* for 30 s.
- **ExoQuick ULTRA**: 100 µL + 30 µL precipitant, on ice for 30 min, 3000*g* for 10 min, then passed through the kit's purification column (Buffer B/A).
- **TEI**: 100 µL + 50 µL PBS + 20 µL precipitant, room temperature for 10 min, 10,000*g* for 5 min.
- **NPR** (Norgen): 100 µL diluted to 3.9 mL water + ExoC, Slurry E resin added, ExoR elution, collected on a mini-filter column.
- **MagNet** (key): MagReSyn SAX (MR-SAX002), equilibrated with 50 mM Bis-Tris pH 6.5 + 150 mM NaCl; plasma mixed 1:1 with 100 mM Bis-Tris Propane pH 6.3 + 150 mM NaCl, bound at room temperature for 30 min, washed 3 times. **Then elution**: 100 µL elution buffer (25 mM Bis-Tris Propane pH 6.5, 1 M NaCl, 0.1% Tween 20), and the eluate then went through 10 kDa buffer exchange. **This paper's MagNet is "separation with SAX beads, elution, then S-Trap digestion", not the same batch of beads digested on-bead by PAC as in Wu / the manufacturer's manual.** This is the single most important sentence when interpreting the number 785.
- **MagCap**: Tim4-PS affinity; plasma first given 5 U heparin to reduce nonspecific binding, 4°C for 1 h, washed 3 times, eluted twice with 50 µL elution buffer. It captures only PS+ EVs, the narrowest subpopulation.

## 3. Characterization and proteomics methods (the unified downstream, which sets the boundary of comparability)

- **NTA**: ZetaView Quatt in scatter mode, 1000-fold dilution, triplicates per sample. Note: scatter mode counts all particles, lipoproteins included, so yield numbers cannot be read directly as EV yield.
- **Simple Western** (Jess automated): 20 µL pooled from each triplicate measured; CD9 additionally measured separately for all 27 samples for reproducibility. Markers: CD9, CD81, Alix, annexin A2, HSPA8, TSG101; contaminants: albumin, ApoA1, ApoE3. RIPA lysis + DTT, 95°C. Loading in Figure 3A was normalized by **the same starting plasma volume**.
- **Proteomics**: S-Trap digestion. EVs equivalent to 3 µL of plasma (made up to 50 µL PBS) + 50 µL 2× lysis buffer (10% SDS, 100 mM TEAB), sonication, TCEP reduction at 55°C, IAA alkylation, phosphoric-acid acidification, 90% methanol binding buffer onto an S-Trap micro column, trypsin/LysC 1 µg, 47°C for 3 h. Evotip loading, Evosep One (8 cm × 150 µm, 1.5 µm C18), 1 µL/min, **21-minute gradient (60 SPD)**, Orbitrap Ascend: MS1 380–985 m/z, 60k, DIA 10 m/z windows, 30k, NCE 25.
- **Search**: DIA-NN 1.8.1 **library-free**, UniProt human database (downloaded 2024-06-07), Trypsin/P, 1 missed cleavage, Cys fixed carbamidomethyl, MBR enabled, gene-level inference. Perseus 1.6.14: >70% valid-value filtering, log2, width normalization, normal-distribution imputation, permutation FDR <0.05, S0 <0.05. The >70% filtering + imputation means a batch of low-abundance proteins is screened out first, so the numbers are conservative.

## 4. Results

### 4.1 NTA: size and yield — selectivity is inversely related to yield

- Mean particle size 90–117 nm. Narrowest distributions: MagCap, NPR, MagNet, DGUC; widest: TEI, ExoQ, qEV, SSEC, UC, with a substantial proportion of >200 nm particles.
- <30 nm non-EV particles (lipoproteins / exomeres / protein aggregates): ExoQ about 4%, TEI about 2%, the rest <1%.
- Yield: selective methods (MagCap, MagNet, NPR, DGUC) gave 1.4–4.7×10⁹ particles per 100 µL plasma, 8–17-fold lower than UC/qEV/SSEC (1.2–8.4×10¹⁰). qEV had the highest yield but about 15% large EVs, SSEC about 17%. UC 1.2×10¹⁰ vs. DGUC 3.5×10⁹; TEI 1.8×10¹⁰ slightly higher than ExoQ 1.4×10¹⁰ (ExoQ has one extra column-purification step).
- Size and yield CVs <20% for all methods. The authors' conclusion sentence: the higher the selectivity, the lower the yield and the more homogeneous the population.

### 4.2 Simple Western: purity

- All 9 methods enriched EV markers relative to neat plasma. CD81 was detected only in NPR, MagNet, and MagCap (the Discussion adds ExoQ — **the text is inconsistent here**, see §6), and its signal was far weaker than CD9; the authors speculate this is related to the use of frozen plasma (freezing lowers CD81).
- With same-volume loading, the strongest marker signals were: qEV, SSEC, ExoQ, MagNet, MagCap.
- Contaminants: albumin highest in UC, TEI, NPR; DGUC removed lipoproteins most thoroughly, and ApoE3 removal was better than ApoA1 removal — density explanation: ApoE3-bearing LDL/VLDL (1.006–1.063 g/mL) differ greatly in density from EVs (1.10–1.21), while ApoA1's HDL (1.063–1.21) overlaps EVs and is hard to separate.
- CD9 reproducibility: ExoQ, TEI, and MagNet had high signals but CVs of about 18–29%; immediately afterwards the text writes MagCap and MagNet CVs <7% — **MagNet appears in both sentences, which contradict each other**, so do not cite this CV set as a conclusion. The NTA CVs (<20%) are cleaner reproducibility evidence.

### 4.3 Proteomics: MagNet 785 is highest, but check the conditions

- Identifications (library-free, 21 min): **MagNet 785 and MagCap 778 highest**; NPR lowest at 570; controls: neat plasma 634, HansaBioMed plasma EV standard 723, cell EV standard 767. All methods >570.
- Except NPR (23%), protein coverage CVs <20% for all methods. PCA clustered by method (PC1+PC2 44.4%), with almost no batch effect among within-method triplicates; neat plasma and standards separated clearly along PC1.
- Volcano plots vs. neat plasma (the original fold wording is vague, written as "~7-fold, log2 scale"; recorded here as the original numbers, not converted): CD9 enriched highest in ExoQ/MagNet/MagCap (about 7-fold), TSG101 highest in qEV/TEI/MagNet (about 6-fold); albumin removal best with DGUC (about 5-fold), qEV about 4-fold, the rest about 3-fold; ApoA1 removal about 2–3-fold for all methods; ApoE removal DGUC/NPR about 4-fold, the rest about 2-fold; ApoB100 in UC/DGUC about 10-fold, the rest about 6-fold.
- ExoCarta comparison: EV-related protein proportions for all 9 methods were >63%; MagNet about 69%, MagCap about 68%, of which 60 belong to the ExoCarta top 100 markers. Method-specific proteins: MagCap unique 28 (12 related to supramolecular fiber organization, 8 to regulation of cell projection, which the authors associate with PS+ EVs and cytoskeleton / motility); MagNet unique 20 (related to neutrophil degranulation and amino acid biosynthesis). The authors themselves only reach the level of functional association, with no validation.

## 5. The most critical passage in the Discussion: why 785 cannot be compared with Wu's 4000+

The authors state explicitly: this study's workflow was optimized for throughput and unbiased comparison, not for maximum coverage. Three variables stacked together determine the numbers:

1. **Gradient**: 21 minutes. Unpublished data from the authors: doubling the gradient takes MagNet from 785 → 1080.
2. **Library building**: library-free. Wu used a spectral library + a 95-minute gradient, >4000 proteins.
3. **Digestion mode**: this paper used elution followed by S-Trap digestion, and **did not use MagNet on-bead digestion**. Unpublished data from the authors: the complete on-bead MagNet protocol, same 21-minute gradient, library-free, gives 1219 proteins (about 55% higher than 785).
4. **Plasma freshness**: the authors say this is the largest factor. Fresh plasma + complete on-bead protocol, 21 minutes, library-free, gives **2492 proteins** (unpublished data).

In other words: 785 (frozen, eluted, S-Trap, 21 min, library-free) → 1219 (on-bead) → 2492 (on-bead + fresh plasma), and only above that comes Wu's spectral library + 95-minute gradient at 4000+. **These four numbers are the same ladder, not four mutually contradictory conclusions.** When citing, the conditions must be carried along; reporting only 785 or only 4000 is misleading.

## 6. Limitations and criticism (authors' own + mine)

Authors' own: ① pooled healthy-donor plasma, so disease status / individual differences (age, sex, metabolism) cannot be extrapolated; ② frozen plasma may introduce aggregation and change surface markers (e.g., CD81 decrease); ③ contaminants were only checked for albumin and apolipoproteins, and non-vesicular particles were not fully evaluated.

My additions:

- **"MagNet" in this paper is only a label for the separation step**. The beads capture and are then eluted, after which they follow the same S-Trap workflow as the other 8 methods. So this paper can answer "which separation method is cleanest under unified digestion", but cannot answer "how deep can the Wu full workflow dig". For eluted samples, short gradients, and library-free searching, 785 is its reference frame.
- **Proteomics loading was normalized by starting plasma volume** (EVs equivalent to 3 µL plasma), not by protein amount or particle number. Low-yield methods are inherently disadvantaged in part, and the coverage numbers mix a yield effect and a purity effect that cannot be separated.
- **Reproducibility is technical replication**: the same pooled plasma split in three; CVs <20% prove workflow stability, not biological replication. In disease cohorts, individual differences will be much larger.
- **Two internal inconsistencies in the text**: the CD81 method list (Results vs. Discussion) and MagNet's CD9 CV (18–29% vs. <7%). They do not affect the main conclusion, but when citing details, use the Results / figures, or simply do not cite them.
- **No cost / time table**: the Discussion says method choice considered sample volume, cost, time, equipment, and reproducibility, but the full text contains no cost / time comparison table; there is also no TEM. DGUC's 18 h + 4 h is basically infeasible in a clinical-throughput scenario, a point made only in passing in the conclusion.
- **Fold wording is not rigorous**: volcano-plot folds are written as "~7-fold, log2 scale", mixing log2 folds and folds, so they cannot be restored to a unified scale; this note only paraphrases the original.

## 7. Implications for practical application

- **The MagNet choice stands**: in the 9-method comparison under a unified workflow, MagNet and MagCap tie for highest purity, narrowest size distribution, and highest coverage, at the cost of 8–17-fold lower yield. For biomarker discovery (small volume, purity needed) this trade-off is right; for yield-demanding scenarios it is not.
- **A yardstick for eluted samples**: if a sample was eluted, run on a short gradient, and searched library-free, identifications around 785 do not mean enrichment failed; look at CD9/TSG101 enrichment and albumin/ApoB removal, not the total. To compare the total with Wu's 4000+, the four conditions — on-bead digestion, long gradient, spectral library, fresh plasma — must first be restored.
- **The three most cost-effective changes, in order of the magnitude in the authors' unpublished data**: ① return to on-bead PAC digestion (785 → 1219, +55%, same gradient and same search); ② fresh plasma (→ 2492, the largest magnitude, but clinical samples may not allow it); ③ long gradient + spectral library (785 → 1080 is only the effect of doubling the gradient; the library gain is in Wu's data). Only stacking all three reaches the 4000+ magnitude; a single change alone cannot get there.
- **MagCap is an alternative, not a replacement**: its coverage is comparable to MagNet, but Tim4 captures only PS+ EVs, a narrower subpopulation; MagNet's electrostatic adsorption spectrum is wider. Switching methods equals switching the biological question, so the subpopulation definition must be fixed before comparing.
- **DGUC is not worth pursuing**: best decontamination, but 22 hours of two rounds of ultracentrifugation + lowest yield make it unsuitable for clinical sample-volume projects.

## 8. Citation and verification

- The full text is freely readable on PMC (CC-BY 4.0); the numbers in this note were checked against the full text, not paraphrased from the abstract.
- Supplementary materials: Simple Western full figures (Fig. S1), clustering figure (Fig. S2), per-method DIA-NN identified protein tables (Table S1, xlsx) — not checked item by item in this deep read; if method-specific protein lists are needed, check Table S1 / PRIDE PXD058948 directly.
