# DPHL v2 — Summary

DIA Pan-Human Library v2 — a human pan-proteome spectral library (Orbitrap data).

## Paper

Xue Z, Zhu T, Zhang F, et al. DPHL v.2: An updated and comprehensive DIA pan-human assay library for quantifying more than 14,000 proteins. *Patterns* 4(7):100792 (2023).
https://doi.org/10.1016/j.patter.2023.100792 · PMID: 37521047 · PMCID: PMC10382975

## Data sources

- 1,608 DDA files: 586 newly acquired (18 tissue types, incl. prostate cancer, liver cancer, triple-negative breast cancer, lung adenocarcinoma, esophageal cancer, GBM, healthy brain, ovarian cancer, cervical cancer, AML/T-ALL plasma, K562, etc.) + 1,022 from DPHL v1
- 24 sample types total; both FFPE and fresh-frozen samples used
- Covers >66.1% of UniProtKB/SwissProt reviewed human proteins

## Build pipeline

- MS-Fragger v3.0 search + Philosopher v3.2.9
- Two fastas: UniProt reviewed (20,350 entries) / isoform (21,997 isoforms, 42,347 entries total)
- Two digestion modes: fully specific / semi-specific (incl. N- and C-terminal semi-tryptic peptides), up to 2 missed cleavages
- Spectrum/peptide/protein three-level FDR < 0.01 (large-sample correction)
- RT alignment with CiRT (endogenous high-abundance conserved peptides via EasyPQP); v1 used synthetic iRT (SiRT)

## Four versions

2 fastas × 2 digestion modes:

| Version | Fasta | Digestion | Precursors | Peptides | Proteins |
|---|---|---|---|---|---|
| RF | reviewed | fully specific | 601,982 | 441,141 | 13,465 |
| RS | reviewed | semi-specific | 772,401 | 588,984 | 13,570 |
| IF | isoform | fully specific | 604,748 | 443,150 | 14,375 |
| IS | isoform | semi-specific | 808,672 | 624,467 | 14,555 |

- Protein counts mostly driven by the fasta; peptide counts mostly driven by digestion mode
- Brain tissue yielded the most proteins; includes 452 FDA-approved drug targets and 100 ovary-enriched proteins
- vs v1: +21.7% protein identifications, +14.2% differential proteins (RF version, colorectal cancer cohort); 1,144 proteins unique to v2

## Access (public, free)

- iProX: **IPX0005714000**
- ProteomeXchange: **PXD039313**
- All four library versions in tsv format, plus reviewed/isoform fastas and raw mzXML data

## Software compatibility

The paper claims compatibility with OpenSWATH, DIA-NN, Skyline and Spectronaut (some need format conversion). DIA-BERT loads DPHL v2 in DIA-NN 1.7.12 for library-based search. But the tsv is the output of the MS-Fragger/Philosopher/EasyPQP pipeline and is not on DIA-NN's "as is" supported list — column-name mapping via `--library-headers` may be needed. RTs in the library are on the CiRT normalized scale; DIA-NN accepts any RT scale as reference.

## v1 reference

Zhu T et al. *Genomics Proteomics Bioinformatics* 18:104–119 (2020).
https://doi.org/10.1016/j.gpb.2019.11.008
1,096 DDA files, 16 cancer sample types; 289,237 precursors / 10,943 proteins. Public and free: iProX **IPX0001400000**.
