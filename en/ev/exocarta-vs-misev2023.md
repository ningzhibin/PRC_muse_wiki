# ExoCarta vs MISEV2023: one is a database, one is a guideline

The two are often mentioned together, but they are **not the same kind of thing**. ExoCarta answers "what molecules have previously been reported in exosomes / small extracellular vesicles, and how often"; MISEV2023 answers "how should an EV study be done and reported". One is a data compendium, the other a consensus guideline. The full text of MISEV2023 (checked 2026-10-28) **does not mention ExoCarta once**; it points readers to UniProt subcellular-location / feature annotations for assigning candidate marker proteins to categories, not to ExoCarta.

| | ExoCarta | MISEV2023 |
|---|---|---|
| What it is | Manually curated cargo database (proteins, RNA, lipids) | ISEV position paper / guideline: minimal information requirements + recommendations |
| Question it answers | Has a given molecule been reported in sEVs before? How frequently? | How should nomenclature, sampling, separation, characterisation, and functional studies be done and reported? |
| Contains data? | Yes, searchable by study / molecule / organism / sample type | No cargo data at all |
| Version | Last published update 2016 (see currency warning below) | Published 2024; predecessors MISEV2014, MISEV2018 |
| Cite as | Keerthikumar et al., *J Mol Biol* 2016;428:688–692 | Welsh et al., *J Extracell Vesicles* 2024;13:e12404 |

## ExoCarta

**History.** First proposed by Mathivanan & Simpson in *Proteomics* (2009); ExoCarta 2012 in the *Nucleic Acids Research* database issue; the 2016 *Journal of Molecular Biology* update (Keerthikumar et al.) — the last published update that could be found. Maintained by the Mathivanan laboratory, La Trobe University.

**Scope.** The homepage now calls it a "small extracellular vesicle (sEV) protein, RNA and lipid database": sEVs comprise 30–150 nm exosomes of endocytic origin and small ectosomes shed from the plasma membrane. Note the tension with MISEV2023 nomenclature: the database is named for "exosomes", but under MISEV2023 that term should only be used when endosomal origin is demonstrated; most entries in practice come from broad EV populations, not strictly proven exosomes.

**Content (2016 snapshot, not current status).** The 2016 paper reports **41,860 proteins, >7,540 RNAs, and 1,116 lipid molecules from >286 studies**, annotated against the ISEV minimal experimental requirements of the time (the MISEV2014 era). The same paper states that nearly 25% of human proteins are known to be secreted via exosomes. Searchable / browsable by gene symbol, protein name, miRNA ID, organism, content type, and sample material; protein–protein interaction networks and pathways are included; the most frequently identified proteins can be downloaded ranked by number of studies and imported into FunRich for enrichment / network analysis.

**Currency warning (checked 2026-10-28).** The ExoCarta homepage shows no counts, version number, or update date, and the statistics page (`ExoCarta_Statistics.html`) returns a 404 / resource-not-found error in a live browser. No later published update was found. So **the 41,860 figure may only be cited as the 2016 snapshot; check the site at the time of use and never present it as the current content**. Most entries are qualitative "has been reported" records from heterogeneous methods: **presence in ExoCarta ≠ marker, ≠ EV-specific, ≠ quantitative enrichment**.

**What "Top 100 / Top 1000" means.** A frequency ranking — how many independent studies reported the protein — not an abundance or enrichment ranking. Example of proper use (Wu et al. 2025, MagNet plasma sEVs): of the Top 1000, 729 were detected, 76% of them enriched relative to unfractionated plasma; of the Top 100, all were detected and 87% enriched. Use it as a frequency reference, then judge your own data by enrichment/depletion versus source material and the MISEV five-category framework.

**Sibling database.** Vesiclepedia (Kalra et al., *PLoS Biol* 2012;10:e1001450; 2019 *Nucleic Acids Research* update) is broader, covering all EV classes (exosomes, ectosomes/microvesicles, apoptotic bodies), >300 studies at launch, with continuous community annotation. EVpedia (Kim et al.) is a separate, independent resource — do not confuse the two.

## MISEV2023

**Source and scale.** Welsh et al., *J Extracell Vesicles* 2024;13(2):e12404, PMID 38326288, PMCID PMC10850029; erratum 2024;13(5):e12451. Three years in the making: a 2020 pre-MISEV survey, a five-author organising committee, a 2022 survey with >1,000 responses, and finally **1,051 co-authors from 53 countries**. Nine sections: nomenclature; non-vesicular extracellular particles (NVEPs, new); collection and pre-processing including bacteria, biofluids, and tissue (new); separation and concentration; characterisation; technique-specific reporting (new); EV release and uptake (new); functional studies; in vivo EV studies (new).

**Nomenclature (§2 — the part that matters most for proteomics reporting).**

- EV definition: particles released from cells, delimited by a lipid bilayer, unable to replicate on their own (no functional nucleus).
- Recommended umbrella term: extracellular particles (EPs), covering EVs plus NVEPs; NVEPs are multimolecular assemblies without a delimiting lipid bilayer (lipoproteins, RNPs, viruses, exomeres/supermeres), which frequently co-isolate with EVs.
- "Exosome" (endosomal origin) and "ectosome"/"microvesicle" (plasma-membrane origin) are **biogenesis terms, discouraged unless subcellular origin can be demonstrated** for the specific preparation; most published work actually studies broad EV populations.
- Operational terms (small EVs usually <200 nm, large EVs >200 nm) are allowed but require caution and an explicit definition, because separation methods yield overlapping size profiles. **sEV ≠ exosome**: small-EV populations contain both small ectosomes and exosomes.

**Characterisation (§5).** Quantitative plus qualitative: report the full particle-size distribution, not just mean/mode/median; total-protein quantification (colorimetric, fluorometric, gel stain, absorbance) **overestimates EV amount because of co-isolated protein** and should not be used as a surrogate for EV concentration — if protein/particle ratios are reported, both original measurements must be given; morphology is best assessed by EM / cryo-EM / AFM (desiccated samples can produce artefactual cup shapes).

**The five-category protein framework (§5.7, Table 3).** MISEV2023 states explicitly that, given EV heterogeneity, **no molecular markers of specific EV subtypes can be recommended**. Claims about EV protein content should be reported with this framework, ideally showing enrichment or depletion of markers versus unfractionated source material:

| Category | Meaning | Examples |
|---|---|---|
| 1. Transmembrane / GPI-anchored proteins (plasma membrane and/or endosomes) | Evidence EVs are present | 1a multi-pass: tetraspanins CD9, CD63, CD81, CD82; 1b single-pass: MHC I/II, integrins, LAMP1/2, TFRC; 1c GPI/lipid-anchored: GPC1, CD73 (NT5E), CD59 |
| 2. Cytosolic proteins | Evidence EVs are present | 2a lipid/membrane-binding: ESCRT (TSG101, CHMP), ALIX (PDCD6IP), VPS4, FLOT1/2, syntenin (SDCBP); 2b promiscuous incorporation: HSPA8, HSP90AB1, ACT, TUB, GAPDH (note: also abundant in NVEPs) |
| 3. Major components of non-EV co-isolated structures (NVEPs) | Purity control | 3a lipoproteins (apolipoproteins); 3b protein / protein–nucleic-acid aggregates: immunoglobulins, UMOD, albumin, YWHA, AGO; 3c exomere/supermere-enriched: HSP90AA/B, TGFBI, LDHA/B |
| 4. Proteins of intracellular compartments other than PM/endosomes | Subtype / pathological state / novel method; optional | Histones, LMNA/C (nucleus); VDAC, CYCS, TOMM20 (mitochondria); CANX, HSP90B1, HSPA5, GOLGA2 (ER/Golgi); MAP1LC3A, ACTN1/4 (autophagosomes/cytoskeleton) |
| 5. Secreted proteins recovered with EVs | Corona or functional component | Apolipoproteins / complement / fibrinogen; cytokines and growth factors (TGFB, IFNG, VEGFA); FN1, collagens, LGALS3BP, AHSG |

Rule: **analyse at least one protein from each of Categories 1, 2, and 3**; Category 4 is optional; Category 5 proteins may bind EVs after release (the EV "corona"). The table gives only a few nominative examples; other proteins are assigned to categories using UniProt "Subcellular location" and "Features" (transmembrane domains, topology).

**Mass spectrometry proteomics (§6.3).** Untargeted analysis (ToF/Orbitrap, discovery, reflects the whole sample composition including contaminants, suited to biomarker discovery) versus targeted analysis (QQQ, presence/absence above a pre-specified threshold and absolute quantification with stable-isotope-labelled peptide standards in a matched matrix). Reporting requirements: follow MIAPE; describe sample preparation step by step; report software and versions, identification/quantification filters, scores and confidence levels, and the quantification method; state the **normalisation basis** (total protein, starting-material volume, or particle count); upload data and metadata to a repository. Targeted analyses additionally require positive and negative controls, at least one quantifier plus one (preferably two) qualifier ion transitions per peptide, linearity and LOD/LOQ defined in a matched matrix, and the target peptide sequences plus the peptide-selection strategy.

**What MISEV is not.** Per the ISEV website: it is not an unreasonable barrier to field entry, not an attempt to stifle innovation, and not irrelevant to non-mammalian or clinical studies. It sets **minimum reporting requirements**; it does not prescribe one separation method as the standard.

## Using the two together (PRC workflow view)

1. **Design and report with MISEV2023**: EV/sEV nomenclature with operational definitions; separation method, starting-material amount, pre-processing, and characterisation (size distribution + morphology + five-category proteins) all recorded; MS reported per §6.3 + MIAPE, with data deposited.
2. **Use ExoCarta as a frequency reference**: compare an identification list against the ExoCarta Top 100/1000 to see the fraction of frequently reported EV-associated proteins — but draw conclusions from **enrichment/depletion relative to source material**, not from the number of identifications.
3. **The test for plasma sEVs**: enrichment of Category 1/2 markers plus depletion of Category 3 (ALB, APOB, etc.) is the evidence; total identification count alone is not. Two examples: Wu et al. 2025 (MagNet same-bead PAC digestion, chromatogram library, 110 min gradient): markers enriched >16-fold, CD9/CD63/SDCBP/NCAM1 >30-fold, ALB depleted ~95%, >4,000 proteins; Suresh & Zhang 2025 (MagNet elution followed by S-Trap digestion, 21 min gradient, library-free): 785 proteins, ~69% ExoCarta-associated, 60 of the Top 100 — **the two numbers are not directly comparable**, because gradient length, library, digestion mode, and plasma freshness all differ.

## Common misuses

- Treating "in the ExoCarta Top 100" as a validated marker — it is only a high-frequency report.
- Calling a preparation "exosomes" without evidence of biogenesis.
- Citing 41,860 proteins as ExoCarta's current content — that is the 2016 figure, and the site's statistics page is dead (404).
- Using total protein as a measure of EV amount, or total identification count as a measure of enrichment.
- Drawing conclusions from a single characterisation technique (MISEV2023 asks for orthogonal evidence).

## References

- Keerthikumar S et al. ExoCarta: a web-based compendium of exosomal cargo. *J Mol Biol* 2016;428:688–692. doi:10.1016/j.jmb.2015.09.019
- Mathivanan S et al. ExoCarta 2012: database of exosomal proteins, RNA and lipids. *Nucleic Acids Res* 2012;40:D1241–D1244.
- Mathivanan S, Simpson RJ. ExoCarta: a compendium of exosomal proteins and RNA. *Proteomics* 2009;9:4997–5000.
- Kalra H et al. Vesiclepedia: a compendium for extracellular vesicles with continuous community annotation. *PLoS Biol* 2012;10:e1001450. doi:10.1371/journal.pbio.1001450
- Pathan M et al. Vesiclepedia 2019: a compendium of RNA, proteins, lipids and metabolites in extracellular vesicles. *Nucleic Acids Res* 2019;47:D516–D519.
- Welsh JA et al. Minimal information for studies of extracellular vesicles (MISEV2023): from basic to advanced approaches. *J Extracell Vesicles* 2024;13:e12404. doi:10.1002/jev2.12404 (PMCID PMC10850029; erratum 2024;13:e12451)
- International Society for Extracellular Vesicles. MISEV. https://www.isev.org/misev
- ExoCarta. http://exocarta.org/ (homepage checked 2026-10-28; statistics page 404, see above)
- Wu CC et al. MagNet: *Nat Commun* 2025;16:5447. doi:10.1038/s41467-025-60595-7 (data PXD042947)
- Suresh PSS, Zhang Q. Comprehensive comparison of methods for isolation of extracellular vesicles from human plasma. *J Proteome Res* 2025;24:2956–2967. doi:10.1021/acs.jproteome.5c00149 (data PXD058948)

*Verification note: the full text of MISEV2023 (PMC10850029) and the ExoCarta homepage were checked line by line on 2026-10-28; the ExoCarta statistics page returned a 404 in a live browser.*
