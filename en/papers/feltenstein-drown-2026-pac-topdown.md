# Deep Read: Protein Aggregation Capture for Top-Down Proteomics

**Source**: Isabella G. Feltenstein, Bryon S. Drown (Department of Chemistry, Purdue University). Preprint, bioRxiv 2026-07-03, DOI: 10.64898/2026.07.02.736076, CC-BY-NC-ND 4.0, **not peer-reviewed**; formally published in *J Proteome Res* 2026, online 2026-10-26, DOI: 10.1021/acs.jproteome.6c00583 — **this deep read covers the full text of preprint v1 and has not checked whether the formal version differs**, so when citing the formal version, the formal version governs. Data: ProteomeXchange / MassIVE, PXD080383; analysis and plotting code: https://github.com/drown-lab/pac-cleanup-tdms; supplementary material includes a step-by-step protocol (S2), lysate preparation (S3–S4), the ContamSpot procedure (S5), and full proteoform hit tables (.xlsx).

**In one sentence**: The SP3 bead logic can be extended to intact proteins: hydroxyl magnetic beads + 70% ACN aggregation, direct elution with 0.5% TFA, no digestion, direct RPLC-MS for top-down analysis. Protein count is comparable to methanol-chloroform-water precipitation (MCW) (231 vs 228), but proteoform count lags by about 20% (580 vs 735), and the bottleneck is incomplete resolubilization of bead-aggregated protein.

## Why do this

- Top-down analysis targets intact proteoforms (PTM positions, stoichiometry, combinations), but it is less sensitive than bottom-up analysis, and sample amounts are often at the µg–mg scale; non-specific surface adsorption during preparation and residual detergent are major sources of loss and interference.
- Targeted top-down analysis (measuring a specific protein family after IP enrichment, e.g. KRAS, α-synuclein, cardiac troponin, apolipoprotein, transthyretin) needs this in particular: IP elution commonly uses SDS (it elutes almost any antigen), but trace SDS suppresses ionization and depresses low-abundance proteoforms; MCW removes SDS, but at sub-µg amounts the precipitate is invisible, making washing and resuspension hard to do. Beads remain visible throughout, which is this paper's selling point.
- Previous uses of SP3 for top-down analysis: carboxylate beads + high-concentration formic acid, or an additional ion-exchange cleanup step; or the gel fractionation + SP3 purification of Takemori et al. 2025 (this paper's ref 5). This paper aims for a single step, direct to the mass spectrometer. Note: the 40% formic acid figure comes from a paraphrase of the abstract; the main text says carboxylate beads require "high-concentration formic acid (e.g. 80%) or dilute SDS resolubilization" — do not mix up the numbers.

## Methods (preprint main text + methods section; all key parameters follow this source)

- Hydroxyl bead method (MagReSyn hydroxyl magnetic beads): sample first reduced with DTT (10 mM, 60 °C, 30 min) → add 100 µg hydroxyl beads + ACN to 70% (v/v), mix at 1000 rpm for 10 min (room temperature) to aggregate → collect beads on a magnetic rack, wash twice with 100% ACN (without disturbing the bead–protein precipitate) → elute with 0.5% TFA, 1000 rpm for 20 min, inject directly.
- Carboxylate bead control (Cytiva): 50% ethanol binding, 80% ethanol wash, 0.5% TFA elution (a different solvent system from the hydroxyl method, **not just a bead swap** — keep this confounder in mind when comparing).
- MCW control: standard protocol, final resuspension in 5% ACN / 0.2% formic acid.
- Standards: Pierce Intact Protein Standard Mix + a four-protein mix (myoglobin, carbonic anhydrase, trypsinogen, ubiquitin, NRTDP standard); standard mix input 4.5 µg.
- Lysate: HeLa, 50 mM HEPES pH 8.5 + 1% SDS + protease inhibitors, probe sonication, clarified to about 2 mg/mL, 30 µL per condition (about 60 µg).
- LC-MS: Vanquish Neo + Orbitrap Exploris 480 (**intact-protein mode**), MAbPac capillary RP column (150 µm × 15 cm), 1.5 µL/min, A = 0.1% FA in water, B = 0.1% FA in ACN, 4–64% B over about 55 min; MS1 120k, 500–2000 m/z, DDA HCD top-3, MS2 60k.
- Database search: Proteome Discoverer 3.0 SP1 + ProSight PD node 4.2, human SwissProt (2023-06-13), high–high, precursor 2.2 Da / fragment 10 ppm, 1% FDR at spectrum, proteoform, and protein levels.
- Version read: full text of the preprint PDF (main text + Table 1 + figure captions; supplementary Figures S4–S8 were seen only as described in the main text, not checked figure by figure).

## Results

### 1. Bead surface chemistry decides success or failure (standards)

- Hydroxyl beads + 0.5% TFA: all standard proteins were detected, and elution spectra were essentially unchanged. S/N of the Protein G [M+23H]23+ precursor: MCW 292.91, hydroxyl beads 212.96, carboxylate beads + 0.5% TFA only 57.51.
- Carboxylate beads require harsh conditions (the text mentions 80% formic acid) to elute protein, but protein stability is limited under such conditions — that is the motivation for switching to hydroxyl beads.

### 2. Dilute-sample recovery: improved, but not significantly

- 200 ng equine myoglobin, 4 ng/µL, in 0.5% SDS, MCW vs hydroxyl SP3, 6 independent replicates each, ProteoQuant quantification (apex + AUC).
- Hydroxyl SP3 was 2.4-fold (AUC) / 3.1-fold (apex) higher than MCW, **but not significant** (figure caption p = 0.12). The conclusion can only be "no sensitivity penalty + easier handling", not "significantly more sensitive". At this concentration the MCW precipitate is invisible, and the procedure itself is hard to carry out.

### 3. SDS removal: removed, but the residual amount cannot be quantified

- ContamSpot colorimetric assay: BSA in 50 mM HEPES + 1% SDS, before vs after cleanup, SDS signal disappeared.
- **Caveat**: TFA itself forms a strong ion pair with the dye O-toluidine blue, and the color intensity of a 0.5% TFA-only control is comparable to that of the hydroxyl SP3 eluate, so **the exact amount of residual SDS cannot be measured** — only qualitatively "removed". When citing, do not write it as "SDS residue below some value".

### 4. Single-shot top-down of HeLa lysate (Table 1, the core table of the paper)

| Cleanup method | Recovery solution | Proteins | Unique proteoforms | Annotated proteoforms | Subsequence proteoforms |
|---|---|---|---|---|---|
| MCW | 5% ACN, 0.1% FA | 228 | 735 | 480 | 429 |
| Cytiva carboxylate | 10% FA | 185 | 544 | 372 | 291 |
| Cytiva carboxylate | 20% FA | 158 | 506 | 337 | 264 |
| Cytiva carboxylate | 0.5% TFA | 204 | 560 | 380 | 311 |
| Cytiva carboxylate | 2% TFA | 105 | 431 | 276 | 262 |
| MagReSyn hydroxyl | 10% FA | 206 | 572 | 405 | 306 |
| MagReSyn hydroxyl | 20% FA | 186 | 470 | 325 | 259 |
| MagReSyn hydroxyl | 0.5% TFA | **231** | **580** | **421** | **314** |
| MagReSyn hydroxyl | 2% TFA | 173 | 486 | 321 | 298 |
| All conditions combined, deduplicated | | 347 | 1385 | 843 | 847 |

- Protein count: hydroxyl beads with 0.5% TFA slightly exceed MCW (231 vs 228), but proteoform count lags by 21% (580 vs 735). **Comparable protein count ≠ comparable proteoform-level performance** — this is the easiest point in the paper to misread.
- Bottleneck evidence: boiling the beads before and after elution for SDS-PAGE (Figure S5) shows protein still on the beads — incomplete resolubilization of aggregated protein, which the authors themselves identify as the limiting factor.
- More acid is not better: 2% TFA and 20% FA both perform worse than 0.5% TFA / 10% FA; high-concentration organic acids also produce more unannotated mass shifts (Figure S8).
- Quality: P-score distributions of shared proteoforms are broadly similar across methods; with FLASHDeconv deconvolution, MCW and hydroxyl beads with 0.5% TFA show the best enrichment of high Q-scores and the most deconvolved masses at 5% FDR. Example: the main proteoform of Acyl-CoA-binding protein, S/N of [M+12H]12+ was MCW 41.78, MagReSyn 215.99, Cytiva 27.79 — the hydroxyl bead spectra have clearly lower noise.
- Retention-time distribution: proteoforms concentrate in the first half of the gradient, correlated with hydrophobicity/size (Figure S4).

### 5. Any bias: no appreciable bias

- GRAVY, pI, average mass: at first glance the alkaline pI bias is caused by histone proteoforms dominating; after removing histones, pI shows the typical bimodal distribution. Between methods, omnibus effect size η² ≤ 0.018 (negligible–small), and pairwise differences still significant after multiple-testing correction have Cliff's δ ≤ 0.25 (small). Conclusion: the proteoforms recovered by PAC are comparable to MCW in mass, charge, and hydrophobicity range.
- Mass shifts (MSTopDiff): common PTMs (methylation, oxidation, acetylation, phosphorylation) were all observed; none of the three main methods introduced artificial modifications; unannotated mass shifts are more frequent at high organic-acid concentrations.

## Mechanistic discussion: PAC vs HILIC — the authors do not settle it

- The mainstream explanation of the SP3 binding mechanism has shifted from HILIC to PAC (protein aggregation capture): aggregation does not depend on bead surface type, and almost any bead works.
- But this paper shows that surface chemistry, bead structure, and recovery solution all affect elution: Cytiva beads are a polystyrene core + magnetite/polymer layer, while MagReSyn beads are a magnetite core with a dendritic polymer exterior — **the effect of surface chemistry is confounded with core structure and cannot be separated**, and the authors acknowledge that more work is needed.
- The effect of organic solvent type/concentration in the aggregation step has also not been systematically evaluated; adding a lyotrope (chaotrope-like promoter) at binding could improve low-abundance recovery, a future direction listed by the authors.

## Limitations (authors' own + reading notes)

- Instrument side: RPLC-MS/MS on the Exploris 480 is limited by ion decay, low HCD dissociation efficiency, and overlapping fragment-ion spectra; individual-ion measurement, ion mobility, other dissociation modes, and gas-phase reaction deconvolution could all deepen proteoform characterization — none of these were done here.
- Only two commercial bead products were tested; other commercial/custom particles might perform better.
- The replicate number for the lysate experiment is not explicitly stated in the main-text methods section (the standard dilution experiment is stated as n=6), so when citing identification numbers, treat them as the magnitude of a single-shot comparison, not as a conclusion with error bars.
- **Reproducibility threshold**: this paper uses the Exploris 480 intact-protein mode, so direct reproduction requires an instrument with that mode. Also: this is denaturing top-down analysis (with SDS), which is a different thing from native MS (which must preserve non-covalent complexes) — conclusions cannot be borrowed between the two.

## Relationship to conventional SP3 (my notes)

- Same origin, different endpoint: both are "beads + organic solvent aggregation, washing, endpoint processing", but this paper's endpoint is intact-protein acid elution directly into the mass spectrometer, not on-bead digestion; the beads (hydroxyl MagReSyn vs carboxylate Cytiva/AMPure), binding solvent (70% ACN vs the conventional 95% ACN recapture), and elution solution (0.5% TFA) all differ, **so this is not a matter of swapping the elution solution in an existing bottom-up SP3 protocol**.
- The most likely useful scenario is targeted top-down / post-IP samples: sub-µg input, SDS elution buffer, invisible precipitate — that is where the bead method's handling advantage is greatest; for discovery single-shot HeLa, the scale is only 231 proteins / 580 proteoforms, so do not expect bottom-up-level identification numbers.
- If trying it in the future: first reproduce the hydroxyl bead vs MCW S/N comparison with a standard protein mix (the Protein G / ACBP examples), then move to real samples; ContamSpot is qualitative only — do not use it to set an SDS residue standard.
