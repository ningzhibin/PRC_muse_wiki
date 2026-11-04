# Deep Read: Rapid multi-omics sample preparation for mass spectrometry (BAMM)

**Source**: Muehlbauer LK, Jen A, Zhu Y, He Y, Shishkova E, Overmyer KA, Coon JJ (University of Wisconsin-Madison, Coon laboratory), *Anal Chem* 2023 (2023-01-17 issue); PMID: 36594155 · PMCID: PMC10026941 · NIHMSID: NIHMS1879550. Interests: Coon is a consultant for Thermo Fisher Scientific; the authors have filed a provisional patent application through WARF covering portions of the method.

**Note**: The full main text has been read; **the SI (NIHMS1879550 supplement .docx) was read on 2026-11-04**, and sample amounts, bead preparation, LC-MS instruments and gradients, and search/identification software have been verified against it — see 'Verified from the SI' at the end. Still unresolved: the exact proteomics gradient profile (the SI only says gradients were optimized from in-house QCs) and the reason for lowering digestion to 60 °C (not stated in the SI); Figures S1–S5 are images in the SI, so panel values were not extracted one by one.

**In one sentence**: This is not a single proteomics method, but an integration of sample preparation extended from SP3 logic to multi-omics: **the same sample, processed once, yields metabolites + lipids + proteins**, with preparation taking about 3 hours and 17 steps according to the authors, compared with an average of 22 hours and 28 steps for published multi-omics methods (a saving of 11 steps and 19 hours). But the "steps/hours" figures use the authors' own counting convention (steps defined to include pipetting, incubation, vortexing, centrifugation, sonication, bead beating, and freeze-thaw cycles), so when citing them, carry that convention — they are not universal timings.

## Final BAMM workflow (main-text methods section, a skeleton that can be copied directly)

1. **Monophasic extraction + on-bead aggregation**: sample + 3:1:1 n-butanol/ACN/H2O (i.e. 60%/20%/20%), plus washed bead stock to a bead:protein ratio of 10:1, final water content 20±1%. Cultured cells are detached with a plastic cell scraper; other samples are vortexed for 10 s; cells and tissues are additionally sonicated in a chilled water bath (10 °C, 5 min, 20 s on / 10 s off, amplitude 30) — sonication serves both lysis and DNA shearing, and the authors state that it improved the proteomics results.
2. **Incubate on ice for 5 min, magnetic rack for 20 s**; the supernatant = metabolites + lipids, aliquoted into autosampler vials and dried with a SpeedVac.
3. **Accelerated on-bead digestion (no washing)**: the bead–protein mixture is resuspended directly in digestion solution — Promega Rapid Digestion Buffer diluted to 75% + 5 mM TCEP + 20 mM CAA; Rapid Trypsin added at a protein:enzyme ratio of 10:1; thermal mixer at 1000 rpm, 60 °C, 40 min. Place on a magnetic rack, recover the supernatant, acidify with TFA to pH ~2, desalt with Strata-X SPE, and dry.
4. Note: the final workflow **removes the wash steps**. The earlier workflow included a 100% ACN wash + 70% EtOH wash after supernatant removal; removing both steps cost only 3% of protein identifications in a mouse brain digest (the same conclusion for plasma is written in the main text as "not shown", with no data given).

Overnight on-bead digestion used for comparison: 50 mM Tris + 10 mM TCEP + 40 mM CAA, trypsin at about 50:1, overnight on a rocker at room temperature. No-bead control: lysis in 6 M GuHCl, precipitation with 90% methanol, resolubilization in 8 M urea, dilution to 1.5 M urea, then overnight digestion.

## Three improvements, each with its evidence and caveats

### 1. Monophasic solvent: 60% n-butanol / 20% ACN / 20% H2O

- Screening range 0–80% n-butanol, water fixed at 20%, ACN making up the balance; at 70% and 80%, slight to moderate phase separation appeared (when phase-separated, the upper layer was used for both metabolite and lipid analysis).
- Optimization conditions were 10 µL human plasma + 500 µL of each solvent, dried and resuspended for analysis; controls were the monophasic MAW solvent (2:2:1 MeOH/ACN/H2O) and the biphasic Matyash system (MTBE/MeOH/H2O 10:3:2.5).
- 60% was the best overall: highest lipid identification count; metabolite counts were similar across 40–70%, but total intensity was slightly higher at 60%.
- vs MAW: markedly more lipids, comparable metabolite count.
- vs MTBE: **without concentration correction, MTBE gave more identifications, because MTBE is more concentrated**; after concentration correction the two were comparable; if the entire monophasic extract is used for only one class of analysis, there was also no difference from MTBE. So the conclusion "comparable to MTBE" depends on concentration normalization, and that must be stated when citing it.
- Class distributions: lipid class proportions were similar to the MTBE organic phase, with both hydrophilic lyso-phospholipids and hydrophobic triglyceride species recovered; for metabolites, MAW recovered a larger proportion of purines, while 60% recovered more fatty acyls; differences from the MTBE aqueous phase were larger, which the authors attribute to partitioning differences between the two phases of the biphasic system.
- Recovery (standards): lipids mean 85±6%, amino acids mean 99±8%; recovery of every amino acid was higher than with the MTBE aqueous phase, tryptophan 51% higher — hydrophobic amino acids are split between the aqueous and organic layers in the biphasic system, a genuine advantage of the monophasic system.
- Quantitative correlation: log2(peak area) correlated strongly across methods, indicating that the monophasic system does not distort the relative proportions of compound classes.

### 2. Beads: the key is "unmodified", not "carboxylate"

Four bead types were tested: 1 µm hydrophilic carboxylate beads (Cytiva), 1 µm hydrophobic carboxylate beads (Cytiva), 3 µm unmodified silica beads (G-Biosciences), and 700 nm unmodified silica beads (Cytiva; the Figure 4 caption identifies them as SeraSil-Mag 700 nm).

- Lipid and peptide identifications were essentially unaffected by bead type; **recovery of the most polar metabolites was hindered by functionalized beads**, which the authors speculate is inadvertent capture of metabolites by the bead surface; 3 µm unmodified beads partially improved this, and 700 nm unmodified beads were comparable to the no-bead control. The same trend was repeated in mouse brain (data in the repository, not shown in the main text).
- Incubation time from 5 to 60 min had almost no effect on recovery (no time dependence); bead:protein ratios of 1:1, 5:1, 10:1, and 20:1 gave similar peptide yields, so the final workflow follows the SP3-recommended 10:1.
- Significance: extending SP3 to multi-omics **changes the selection criterion for bead surface chemistry** — not protein binding efficiency, but whether metabolites are stolen by the bead surface. The protein aggregation mechanism itself remains aggregation under high organic solvent (citing SP3 reference 33; the authors state that proteins are not influenced by bead properties), so "unmodified beads are best" must not be misread as "unmodified beads bind proteins better".

### 3. Accelerated digestion: Rapid Trypsin, 60 °C, 40 min, shaking throughout

- Compressing overnight digestion to 40 min required two changes: shaking throughout digestion (about 35% higher than without shaking), and lowering the temperature from the Rapid Trypsin standard of 70 °C to 60 °C (Figure S4a; the reason for lowering the temperature is stated neither in the main text nor in the SI, so no reason is invented here).
- Compared with overnight digestion with and without beads, there was no loss of depth or quality. The main text's wording on one cysteine-related metric is ambiguous: semi-tryptic rate and sequence coverage are described as comparable (about <1% and ~30%, respectively) — whether <1% is an absolute value or a difference is not developed in the main text and can only be settled by Figure S4b, so do not cite <1% as a definitive semi-tryptic rate.
- Missed cleavages: the rapid on-bead workflow showed **10–20% fewer** than overnight digestion without beads, consistent with previous reports for SP3 carboxylate bead workflows.
- Peptide hydrophobicity bias: GRAVY distributions were broadly similar, but the bead workflow extracted hydrophilic peptides to a greater extent; the driver is the uniquely identified peptides — SP3-only peptides tended to be more hydrophilic, while solution-digestion-only peptides tended to be more hydrophobic; **all tested bead types showed this bias**, not just the 700 nm beads. Protein identification overlap is described in the main text as about 80% between two replicates without match-between-runs and about 95% with match-between-runs (whether the comparison is between replicates or between methods is not fully developed in the main text, so cite it with that uncertainty).

## Multi-sample validation (Figure 5, means of technical replicates, n=3)

Order: plasma, yeast, mouse adipocytes from culture plates, mouse brain, HEK293 cell pellets.

- Metabolites: 67, 152, 61, 90, 51
- Lipids: 260, 139, 412, 430, 445
- Proteins: 515, 3578, 4322, 5197, 5552

Caveats:

- The metabolite numbers look low; the authors explain that identification filters were strict (cosine similarity threshold of 80). The "Published Data" columns on the left of Figure 5 are numbers carried directly from the results sections of those publications, and the Kang et al. metabolite count was further reduced to match this paper's criteria — **cross-paper comparison conventions are not uniform, so Figure 5 can only be read at the level of magnitude**.
- Validation used technical replicates, not biological replicates or a large cohort; "magnetic bead methods are potentially more amenable to robotic automation and multi-well plate formats" is only a statement of potential — this paper contains no automation validation.
- Protein depth (plasma 515) cannot be directly compared with Astral DIA depth (thousands): this paper's proteomics data were acquired on an Orbitrap Eclipse and searched with MaxQuant 1.5.2.8/Andromeda/LFQ (default parameters, match-between-runs off by default), lipids/metabolites on a Q Exactive HF, from 5 µL of plasma, and the exact proteomics gradient profile is not given in the SI.

## Lessons for a core facility (my judgment, not the paper's conclusions)

- **Same origin as conventional SP3, but with two hard differences**: (a) beads changed to 700 nm unmodified silica beads — an advantage only when "the same batch of samples is also needed for metabolomics/lipidomics"; there is no evidence that unmodified beads are better for protein-only workflows; (b) digestion uses Promega Rapid Trypsin proprietary buffer + thermostable enzyme, 60 °C for 40 min, which cannot be transferred directly onto a conventional trypsin 37 °C overnight workflow; the effect of 60 °C on heat-labile modifications is not discussed in this paper.
- **Relationship to the One-Tip plasma method**: both follow a "fewer steps, single container" idea, but in opposite directions — BAMM splits one sample into three omics, with the protein portion still requiring SPE desalting and drying before injection; One-Tip digests plasma directly inside a C18 tip for injection. BAMM's "3 hours" means only that preparation is complete and ready for analysis; LC-MS time is additional — do not read it as 3 hours to three-omics data.
- **Most direct lesson**: if a client in the future brings precious samples requesting metabolites + lipids + proteins from the same batch, BAMM is a ready-made framework; but when the core does not currently offer metabolomics/lipidomics services, borrowing just "unmodified beads + accelerated digestion" needs separate validation on one's own samples and cannot be copied as a whole workflow.

## Verified from the SI (added 2026-11-04, NIHMS1879550 supplement)

- **Sample amounts**: mouse brain, 15±2 mg of frozen pulverized tissue per tube; plasma, 5 µL per extraction (pooled BioIVT plasma, NaHep Lot HMN378062, for routine experiments; NIST SRM 1950 for Figures 5 and S5); yeast, about 500 µg of protein per pellet; adipocytes and HEK293, no amounts given, only culture and harvest conditions.
- **Bead preparation**: Cytiva SeraSil-Mag 700, taken in a quantity sufficient for a 10:1 bead:protein ratio, washed once or twice with Nanopure water on a magnetic rack, reconstituted in Nanopure water, and stored at 4 °C; sample protein amounts estimated from prior knowledge or measured on a NanoDrop at 280 nm ("1 Abs = 1 mg/mL").
- **Lipidomics LC-MS**: Vanquish autosampler + Waters Acquity CSH C18 (2.1×100 mm, 1.7 µm, 50 °C), 400 µL/min, about a 28-minute gradient; Q Exactive HF with HESI-II, polarity switching Top2, MS1 at 30k, 200–2000 m/z; identification with Compound Discoverer 2.1+ and LipiDex.
- **Metabolomics LC-MS**: Vanquish autosampler + Millipore SeQuant ZIC-pHILIC (2.1×100 mm, 5 µm, 50 °C), 150 µL/min, about a 35-minute gradient; Q Exactive HF, polarity switching Top10, MS1 at 60k, 70–900 m/z; identification with TraceFinder 4.1 + Compound Discoverer 3.3, with targeted features listed in Table S1 (m/z and retention times).
- **Proteomics LC-MS and search**: UltiMate WPS-3000RS autosampler, 1 µg of peptides onto an in-house packed PicoFrit column (30 cm, 1.7 µm BEH C18, 75×360 µm, 50 °C), 300 nL/min; Orbitrap Eclipse, MS1 at 240k, 300–1350 m/z, 1-second cycle, ion-trap turbo MS/MS, NCE 25%, APD on; search with MaxQuant 1.5.2.8 + Andromeda, LFQ, default parameters, UniProt canonical + isoform databases, match-between-runs off unless noted.
- **Other**: internal standards were 5 µL SPLASH Lipidomix + 5 µL 13C/15N amino acid mixture (diluted 1:100); the monophasic/biphasic dilution correction divided the summed phase volumes by the volume of the phase used for analysis and normalized across samples (biphasic volumes measured with a glass Hamilton pipettor); technical replicates n=3, averaged; raw data deposited in MassIVE under MSV000089241.

## Still unverified

- Raw data in the repository (mouse brain bead-type experiments, supplementary n-butanol extraction data) not checked.
- The specific reason for lowering accelerated digestion to 60 °C, the definition of the semi-tryptic <1% figure, and the definition of the 80%/95% protein overlap figure: unclear in the main text and not stated in the SI either; Figure S4 is an image, and its panel values were not extracted.
- The exact proteomics gradient profile: the SI only says gradients were optimized from in-house quality controls, with no gradient table.
