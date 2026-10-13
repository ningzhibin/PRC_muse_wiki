# Pooled Multi-Enzyme Single-Shot DIA: One Pooled-Digest Acquisition + Union Library, One Search

## The recipe (the Richards recipe)

1. Split the same sample into aliquots and digest each with a complementary enzyme (trypsin, AspN, GluC, etc.); quench after digestion.
2. Pool the finished digests proportionally into one tube.
3. One DIA acquisition (one file per sample).
4. Search once against a **union library**, with one round of FDR control.

Key point: do not search the same file once per enzyme and then merge the results (triple searching plus merging three separately FDR-filtered lists).

## Why per-enzyme searching and a union-library search are nearly equivalent in DIA

DIA searching is peptide-centric: searching means extracting chromatographic and fragment evidence for each peptide in the library from multiplexed MS2. Enzyme identity only matters at the step that **defines the candidate peptide set**; once that set is fixed, the search no longer knows which enzyme produced each peptide. Per-enzyme searching (three candidate sets) versus a union-library search (the union of the same sets) evaluates the same peptides against the same data — the results are essentially equivalent.

The real difference is FDR bookkeeping: with separate searches each filtered at 1% FDR, the union of three 1% lists has a true FDR inflated above 1%; with a union-library search, a single target–decoy calibration gives you what you report. The union-library search is not "equivalent but more work" — it is **the clean implementation of the same thing**. This is why Richards / Fossati went with a combined library.

## Implementation detail: what "union" means exactly

A union library must be the **union of candidate peptide sets** built separately per enzyme (in-silico digestion per enzyme, or empirical DDA-based library construction, then merged).

It must NOT be: one in-silico digest with all cleavage motifs stacked in the motif list. Tools like DIA-NN treat every motif listed in `--cut` as "cleavable here," which produces **mixed-termini peptides** (tryptic at one end, GluC at the other) — such peptides cannot exist in a real pooled sample (the digests were quenched before mixing; no peptide ever met two enzymes), so you would inflate the search space for nothing and pay an unnecessary FDR tax.

The only remaining value of separate searches is per-digest QC and seeing each enzyme's contribution; you can partly recover that by tagging each peptide in the library with its source enzyme.

## Key literature

- **Richards AL et al.** *J Proteome Res* 2022;21(4):1124–1136. DOI 10.1021/acs.jproteome.1c00960. Dataset PXD027242. Trypsin/AspN/GluC digests pooled, one DIA run: versus trypsin alone, similar protein identifications, +63% peptides, +45% non-redundant amino acids; non-tryptic peptides support non-canonical isoform calls.
- **Fossati A et al.** *J Proteome Res* 2021;20(8):4031–4040. DOI 10.1021/acs.jproteome.1c00357. Human plasma, timsTOF dia-PASEF. Three separate runs merged in silico: 14,665 peptides / 636 proteins; single pooled run: 10,572 peptides / 489 proteins (~73% peptide recovery), but more complete features and protein CVs about 8 percentage points lower.
- **Dator RP et al.** *Anal Chem* 2014. DOI 10.1021/ac404020j. Precursor: trypsin/chymotrypsin/Asp-N/Glu-C digests pooled, single LC-MSᴱ acquisition, single library-free search. Note: MSᴱ is not windowed DIA, and the search was configured as "trypsin primary + nonspecific," not an explicit union library — a precursor rather than a strict match.
- Search note (2026-10-12): no other publications strictly matching "pool → single run → one union-library search" were found beyond the two above; none of the 18 papers citing Richards or the 20 citing Fossati adopted the design. Gao et al. *Anal Chem* 2020 "Multi-in-One" describes multi-enzyme one-shot DIA phosphoproteomics (~30,000 phosphosites) in its abstract, but the full text is paywalled — it is unverified whether the digests were pooled from separate aliquots or co-digested in one pot; not counted.
