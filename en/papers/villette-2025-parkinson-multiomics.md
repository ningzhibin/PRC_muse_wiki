# Deep Read: Villette et al. 2025 Parkinson's Multi-omics — the Metaproteomics (MP) Layer Dissected

**Source**: Villette et al. (Luxembourg LCSB / Wilmes group; Ben Kunath as co-author), *Microbiome* 2025, 13:200 (DOI: 10.1186/s40168-025-02227-2; PMC: PMC12502400; PMID: 41053825). A Correction dated 2025-11-07 (10.1186/s40168-025-02295-4) only updated Data Availability.

**Data**: MP raw data PRIDE **PXD031457**; MG/MT in SRA BioProject **PRJNA782492**; integration pipeline IMP code at gitlab.lcsb.uni.lu/IMP/imp3.

**In one sentence**: This paper's MP layer is the classic reference for understanding a metaproteomics method baseline — the per-sample matched database idea is right, but Sipros + spectral counting + about 3.4k proteins/sample depth is only baseline level today.

## 1. Study Design

- **Cohort**: Luxembourg DeNoPa cohort, three groups — healthy controls (HC), iRBD (prodromal stage), PD (idiopathic Parkinson's disease).
- **Four omics**: metagenomics (MG), metatranscriptomics (MT), metaproteomics (MP), meta-metabolomics (MM), integrated via IMP (Integrated Meta-omics Pipeline) and connected at the KEGG level.
- MP performed by: the Jehmlich / von Bergen team at UFZ.

## 2. MP Methods (the Focus)

- **Sample preparation**: 20 µL protein extract processed by **SP3** (carboxylate magnetic beads, DTT reduction + IAA alkylation, trypsin 1:50 overnight), **including post-digestion peptide recapture** — ACN added to >95% so peptides rebind to the beads, followed by two-step elution: a high-pH fraction (87% ACN, 10 mM ammonium formate pH 10) + an aqueous fraction (2% DMSO). **Note (checked 2026-10-25)**: Villette used two-step elution, but PRIDE PXD031457 has only 128 RAW files, filenames carry no f1/f2 suffix, and the count is close to the 122 samples — pointing to the two eluates being pooled for one injection per sample; this differs from Radushev et al. 2024 (Sample_f1/_f2 injected separately) — "two-step elution" does not equal "two injections as fractions."
- **Acquisition**: **Q Exactive HF + TriVersa NanoMate (LC-chip mode)**, trap column + 25 cm PepMap analytical column; MS1 120k, 350–1550 m/z. **The Methods only go as far as MS1; gradient length and MS/MS parameters are missing from the full text** — a reporting completeness gap.
- **Database search**: **Sipros v1.1** (an in-house UFZ tool); databases **built per sample** — proteins predicted from the same sample's MG/MT co-assembly (IMP) (average **7.2×10⁵ entries/sample**) + cRAP + the human UniProt reference proteome + reversed decoys. Old-school parameters: MS1 initial tolerance 1 Da, post-filter 0.05 Da, MS2 0.01 Da; ≥1 unique peptide/protein, peptide FDR dynamically adjusted to reach **1% protein FDR**; quantification by **spectral count**; inclusion threshold **≥2,000 proteins/sample**.

## 3. Output and Performance — Taxonomy Layer Weak, Functional Layer Decent

- **Shallow and unstable depth**: average 4.1×10⁴ spectra per sample, identifying **3.4×10³ ± 1.7×10³ proteins** (the standard deviation is half the mean); of 122 samples, MP retained only **104** (42 HC / 22 iRBD / 40 PD), the rest excluded for low identifications.
- **taxMP the weakest in the field**: between-group β diversity did not separate (PERMANOVA **p=0.35, R²=0.017**, at the bottom together with taxMG and funMG), and the main text reports no MP-level significantly different taxa.
- **funMP better**: α diversity significantly higher in PD; β separation significant (**p=0.008, R²=0.041**, second only to funMT's 0.043), and together with funMT it is one of only two layers that distinguish PD vs iRBD; not confounded by sex/age/constipation.
- The paper's headline biology (elevated β-glutamate, downregulated flagellar/chemotaxis transcription) is driven by MM + MT; MP plays a supporting role.

## 4. Positioning of the MP Layer: Integration Touched Low-Abundance Archaea, Single-Layer Statistics Do Not Stand

In the MOFA integration analysis, disease factor F1 was enriched on the PD side for *M. smithii* and "archaeal proteins and genes (based on MP and MG data)" — the original method touched the protein signal of *Methanobrevibacter smithii* at the integration level, but taxMP single-layer statistics do not stand. This is precisely the typical profile of such baseline methods: integration can see a signal, but the single-layer methodology is not yet stable enough, and this is where modern workflows have room to improve.

**In one sentence**: This paper's MP is "classic practice under the 2018-era IMP framework" — the per-sample matched database idea is right, but Sipros + spectral counting + about 3.4k proteins/sample depth is only baseline level today.
