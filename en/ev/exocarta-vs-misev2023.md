# ExoCarta, Vesiclepedia vs MISEV2023: two databases, one guideline

The three are often mentioned together, but they are **not the same kind of thing**. ExoCarta and Vesiclepedia are both data compendia, answering "what molecules have previously been reported in EVs"; MISEV2023 is a consensus guideline, answering "how should an EV study be done and reported". The full text of MISEV2023 (checked 2026-10-28) **does not mention ExoCarta or Vesiclepedia once**; it points readers to UniProt subcellular-location / feature annotations for assigning candidate marker proteins to categories, not to either database.

| | ExoCarta | Vesiclepedia | MISEV2023 |
|---|---|---|---|
| What it is | Manually curated cargo database (proteins, RNA, lipids) | Manually curated + community-annotated cargo database (proteins, RNA, lipids, metabolites), with experimental quality information | ISEV position paper / guideline: minimal information requirements + recommendations |
| Scope | Exosomes / sEVs | **All EV classes** (exosomes, ectosomes/microvesicles, apoptotic bodies, plus newer EP categories) | All EV/EP research |
| Question it answers | Has a given molecule been reported in sEVs before? How frequently? | Has a given molecule been reported in each EV class? What were the source studies' separation/characterisation methods and reporting transparency? | How should nomenclature, sampling, separation, characterisation, and functional studies be done and reported? |
| Contains data? | Yes, searchable by study / molecule / organism / sample type | Yes, filterable by sample source, separation method, EV subtype, and organism | No cargo data at all |
| Version | Last published update 2016 (see currency warning below) | Last published update 2019 (see currency warning below) | Published 2024; predecessors MISEV2014, MISEV2018 |
| Cite as | Keerthikumar et al., *J Mol Biol* 2016;428:688–692 | Pathan et al., *Nucleic Acids Res* 2019;47:D516–D519 | Welsh et al., *J Extracell Vesicles* 2024;13:e12404 |

## ExoCarta

**History.** First proposed by Mathivanan & Simpson in *Proteomics* (2009); ExoCarta 2012 in the *Nucleic Acids Research* database issue; the 2016 *Journal of Molecular Biology* update (Keerthikumar et al.) — the last published update that could be found. Maintained by the Mathivanan laboratory, La Trobe University.

**Scope.** The homepage now calls it a "small extracellular vesicle (sEV) protein, RNA and lipid database": sEVs comprise 30–150 nm exosomes of endocytic origin and small ectosomes shed from the plasma membrane. Note the tension with MISEV2023 nomenclature: the database is named for "exosomes", but under MISEV2023 that term should only be used when endosomal origin is demonstrated; most entries in practice come from broad EV populations, not strictly proven exosomes.

**Content (2016 snapshot, not current status).** The 2016 paper reports **41,860 proteins, >7,540 RNAs, and 1,116 lipid molecules from >286 studies**, annotated against the ISEV minimal experimental requirements of the time (the MISEV2014 era). The same paper states that nearly 25% of human proteins are known to be secreted via exosomes. Searchable / browsable by gene symbol, protein name, miRNA ID, organism, content type, and sample material; protein–protein interaction networks and pathways are included; the most frequently identified proteins can be downloaded ranked by number of studies and imported into FunRich for enrichment / network analysis.

**Currency warning (checked 2026-10-28).** The ExoCarta homepage shows no counts, version number, or update date, and the statistics page (`ExoCarta_Statistics.html`) returns a 404 / resource-not-found error in a live browser. No later published update was found. So **the 41,860 figure may only be cited as the 2016 snapshot; check the site at the time of use and never present it as the current content**. Most entries are qualitative "has been reported" records from heterogeneous methods: **presence in ExoCarta ≠ marker, ≠ EV-specific, ≠ quantitative enrichment**.

**What "Top 100 / Top 1000" means.** A frequency ranking — how many independent studies reported the protein — not an abundance or enrichment ranking. Example of proper use (Wu et al. 2025, MagNet plasma sEVs): of the Top 1000, 729 were detected, 76% of them enriched relative to unfractionated plasma; of the Top 100, all were detected and 87% enriched. Use it as a frequency reference, then judge your own data by enrichment/depletion versus source material and the MISEV five-category framework.

**Relationship to Vesiclepedia.** See the next section: a sibling database maintained by the same group (La Trobe, Mathivanan laboratory), broader in scope. EVpedia (Kim et al.) is a separate, independent resource — do not confuse the two.

## Vesiclepedia

**History.** Launched by Kalra et al., *PLoS Biol* 2012;10:e1001450, explicitly as "continuous community annotation" — a response to the finding that >50% of databases were not regularly updated and >20% of database links were dead — with a community-annotation project to prevent database decay; updated in *Nucleic Acids Research* in 2019 (Pathan et al.) — the last published update that could be found. Maintained by the same group as ExoCarta (Mathivanan laboratory, La Trobe University; the Ghent University Hendrix laboratory co-authored the 2019 version).

**Scope: broader than ExoCarta.** The 2019 paper states it directly: compared with the exosome-focused ExoCarta, Vesiclepedia catalogues proteins, RNAs, lipids, and metabolites identified in **all classes of EVs** — exosomes, ectosomes or shedding microvesicles, and apoptotic bodies — from both prokaryotic and eukaryotic sources, and from published and unpublished studies (unpublished data can stay private until publication). The homepage wording checked on 2026-10-28 has kept pace with the MISEV2023 era: the subtype list now also includes migrasomes, exophers, and large oncosomes, and exomeres/supermeres are listed as extracellular particles (EPs).

**Content (2019 snapshot, not current status) — mind the "entries" vs "unique molecules" distinction.** Table 1 of the 2019 paper: **1,254 EV studies; 349,988 protein entries, but 32,267 unique proteins; 38,146 RNA entries (27,646 mRNA entries / 17,005 unique mRNAs; 10,520 miRNA entries / 2,431 unique miRNAs); 639 lipid/metabolite entries; 41 organisms, 172 sample sources**. Of the 1,254 studies, 648 were manually annotated and the rest contributed by the community. An "entry" counts one report in one study, so **349,988 must never be compared directly with ExoCarta's 41,860 proteins**. By isolation method: ultracentrifugation in 58% of studies (>73% for differential centrifugation counting combined methods), commercial kits/columns in ~7%.

**Two features Vesiclepedia has that ExoCarta does not.**

- **EV-METRIC / EV-TRACK links**: each study's experiment page carries an EV-METRIC score — a measure of the transparency of reporting experimental meta-annotations, based on the isolation and characterisation parameters reported — linked to the EV-TRACK website explaining how the score was reached. This is a ready-made tool for judging a historical cargo record: a protein record from a high-EV-METRIC study is worth more than one from a study that reported only "ultracentrifugation + Western blot".
- **FunRich plugin + filtered downloads**: the whole database can be auto-updated through the FunRich plugin and compared with a user's own dataset by Venn diagrams or enrichment analysis; downloads can be filtered by sample source (e.g. cancer or stem cells), isolation method (e.g. density-gradient centrifugation), EV subtype (e.g. ectosomes), and/or species. For the PRC, that means building a reference set from studies matching one's own sample type and separation method, rather than comparing against all EV studies pooled together.

**The Top 100 warning — written by the authors themselves.** The 2019 paper's "Pitfalls" section: Vesiclepedia's Top 100 proteins "merely means that these 100 proteins are most often reported in EV studies and does not warrant them to be called as EV markers or enriched proteins. While some of the proteins in the list are *bona fide* EV enriched proteins, others could be experimental artefacts and could be of low abundance in EVs." The same warning applies to ExoCarta's Top 100/1000. The authors also stress that database quality is only as good as the source studies, that EV terminology lacks global consensus, and that protocols vary between laboratories, so cross-study comparison is not trivial.

**Currency warning (checked 2026-10-28).** The Vesiclepedia website (microvesicles.org) opens normally and its homepage wording is current, but like ExoCarta's it shows no counts, version number, or update date, and no later published update was found. So **the 2019 figures may only be cited as a snapshot; check the site at the time of use**. Compared with ExoCarta, Vesiclepedia's site is at least alive (ExoCarta's statistics page is a 404) — but "alive" does not mean "up to date".

## ExoCarta vs Vesiclepedia: which one?

| | ExoCarta | Vesiclepedia |
|---|---|---|
| Look up | sEV/exosome cargo, frequency reference (Top 100/1000 most used) | Cargo across EV classes, filterable by subtype / method / organism |
| Metabolites | Not included | Included (639 lipid/metabolite entries combined, 2019) |
| Experimental quality information | Entries annotated against MISEV2014-era minimal requirements | Separation/characterisation methods + EV-METRIC score + EV-TRACK link |
| Interaction / pathway analysis | Protein–protein interaction networks and pathways | FunRich enrichment/network analysis (via plugin) |
| Last published update | 2016 | 2019 |
| Website status (2026-10-28) | Homepage opens, statistics page 404 | Homepage opens, no counts shown |

In practice the two are not an either/or choice: use Vesiclepedia to find comparable historical studies filtered to your sample type and separation method, then use ExoCarta's Top 100/1000 as the sEV frequency reference; a conclusion is firmer when both "most often reported" lists agree. Neither database provides quantitative enrichment information — enrichment is always judged from one's own control experiment.

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

## Using the three together (PRC workflow view)

1. **Design and report with MISEV2023**: EV/sEV nomenclature with operational definitions; separation method, starting-material amount, pre-processing, and characterisation (size distribution + morphology + five-category proteins) all recorded; MS reported per §6.3 + MIAPE, with data deposited.
2. **Use Vesiclepedia to find comparable historical studies**: filter by sample source, separation method, EV subtype, and organism, and use EV-METRIC to judge the reporting transparency of those studies; do not take whole-database frequencies as expected values for your own preparation.
3. **Use ExoCarta as an sEV frequency reference**: compare an identification list against the ExoCarta Top 100/1000 to see the fraction of frequently reported EV-associated proteins — but draw conclusions from **enrichment/depletion relative to source material**, not from the number of identifications.
4. **The test for plasma sEVs**: enrichment of Category 1/2 markers plus depletion of Category 3 (ALB, APOB, etc.) is the evidence; total identification count alone is not. Two examples: Wu et al. 2025 (MagNet same-bead PAC digestion, chromatogram library, 110 min gradient): markers enriched >16-fold, CD9/CD63/SDCBP/NCAM1 >30-fold, ALB depleted ~95%, >4,000 proteins; Suresh & Zhang 2025 (MagNet elution followed by S-Trap digestion, 21 min gradient, library-free): 785 proteins, ~69% ExoCarta-associated, 60 of the Top 100 — **the two numbers are not directly comparable**, because gradient length, library, digestion mode, and plasma freshness all differ.

## Common misuses

- Treating "in the ExoCarta/Vesiclepedia Top 100" as a validated marker — it is only a high-frequency report, and the Vesiclepedia authors say so themselves in the paper.
- Comparing Vesiclepedia's 349,988 protein "entries" with ExoCarta's 41,860 proteins — the former counts study×protein occurrences, the latter unique molecules; different denominators.
- Calling a preparation "exosomes" without evidence of biogenesis.
- Citing 41,860 proteins as ExoCarta's current content — that is the 2016 figure, and the site's statistics page is dead (404); the same year-labelling applies when citing Vesiclepedia's 2019 figures.
- Using total protein as a measure of EV amount, or total identification count as a measure of enrichment.
- Drawing conclusions from a single characterisation technique (MISEV2023 asks for orthogonal evidence).

## References

- Keerthikumar S et al. ExoCarta: a web-based compendium of exosomal cargo. *J Mol Biol* 2016;428:688–692. doi:10.1016/j.jmb.2015.09.019
- Mathivanan S et al. ExoCarta 2012: database of exosomal proteins, RNA and lipids. *Nucleic Acids Res* 2012;40:D1241–D1244.
- Mathivanan S, Simpson RJ. ExoCarta: a compendium of exosomal proteins and RNA. *Proteomics* 2009;9:4997–5000.
- Kalra H et al. Vesiclepedia: a compendium for extracellular vesicles with continuous community annotation. *PLoS Biol* 2012;10:e1001450. doi:10.1371/journal.pbio.1001450
- Pathan M et al. Vesiclepedia 2019: a compendium of RNA, proteins, lipids and metabolites in extracellular vesicles. *Nucleic Acids Res* 2019;47:D516–D519. doi:10.1093/nar/gky1029 (PMCID PMC6323905)
- Vesiclepedia. http://www.microvesicles.org/ (homepage checked 2026-10-28; no counts or update date shown, see above)
- Welsh JA et al. Minimal information for studies of extracellular vesicles (MISEV2023): from basic to advanced approaches. *J Extracell Vesicles* 2024;13:e12404. doi:10.1002/jev2.12404 (PMCID PMC10850029; erratum 2024;13:e12451)
- International Society for Extracellular Vesicles. MISEV. https://www.isev.org/misev
- ExoCarta. http://exocarta.org/ (homepage checked 2026-10-28; statistics page 404, see above)
- Wu CC et al. MagNet: *Nat Commun* 2025;16:5447. doi:10.1038/s41467-025-60595-7 (data PXD042947)
- Suresh PSS, Zhang Q. Comprehensive comparison of methods for isolation of extracellular vesicles from human plasma. *J Proteome Res* 2025;24:2956–2967. doi:10.1021/acs.jproteome.5c00149 (data PXD058948)

*Verification note: the full text of MISEV2023 (PMC10850029), the ExoCarta homepage, the Vesiclepedia homepage, and the full text of Pathan et al. 2019 (PMC6323905) were checked line by line on 2026-10-28; the ExoCarta statistics page returned a 404 in a live browser.*
