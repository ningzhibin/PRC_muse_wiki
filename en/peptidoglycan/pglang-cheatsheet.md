# PGLang cheat sheet, for proteomics people

## Reference

Alamán-Zárate MG, Rady BJ, Ledermann R, et al. A software tool and strategy for peptidoglycomics, the high-resolution analysis of bacterial peptidoglycans via LC-MS/MS. *Commun Chem* 8:91 (2025).
https://doi.org/10.1038/s42004-025-01490-6

Syntax documentation: https://github.com/mesnage-org/pgfinder/blob/HEAD/lib/docs/pglang.md
Web tools: https://mesnage-org.github.io/pgfinder/ (Mass Calculator, Fragment Generator)

## In one sentence

PGLang is a "glycan prefix + modified peptide sequence", plus a small graph syntax for cross-links. If you do proteomics, read it like a modified peptide string with a glycosylated N-terminus.

## 1. Basic skeleton: `glycan-peptide`

`gm-AEJA` = GlcNAc–MurNAc + Ala–Glu–DAP–Ala

- **Glycans (lowercase)**: `g` = GlcNAc, `m` = MurNAc, `x` = unknown monosaccharide
- **Peptide (uppercase)**: one-letter codes as in proteomics, **except five special letters**: `J` = meso-diaminopimelic acid (DAP), `B` = diaminobutyric acid, `O` = ornithine, `U` = homoserine, `Z` = threo-3-hydroxyglutamic acid; `X` = unknown amino acid
- Note: `E` in the stem is **γ-D-glutamic acid** (not α-linked); PGLang does not encode L/D — stereochemistry is handled by the PGLang→SMILES translator

## 2. Modifications: round brackets, immediately after the modified residue

`gm(Anh, Glyc)-AEJ(Am)A` — MurNAc carries Anh and Glyc, J carries amidation; multiple modifications are comma-separated.

| Symbol | Name | Mass shift |
|---|---|---|
| Ac | O-acetylation (6-position hydroxyl) | +42.0106 |
| Am | Amidation (sidechain carboxyl) | −0.9840 |
| Anh | 1,6-anhydro (reducing-end MurNAc) | −18.0106 |
| DeAc | De-N-acetylation | −42.0106 |
| Glyc | Glycolylation (MurNAc N-acetyl → N-glycolyl) | +15.9949 |
| Poly | Wall polymer linkage (phosphate) | +77.9507 |
| Red | Reduced (NaBH₄ reduction, MurNAc → muramitol) | +2.0157 |

## 3. Lateral chains: square brackets, on diamino/dicarboxylic residues

`gm-AQK[GGGGG]A` — the staphylococcal pentaglycine bridge on Lys, then the stem continues with `A`.

A Lys-type stem with a D-Asp cross-bridge is written as `gm-AEK[D]A` (bracket on K, D inside). Paste into the Mass Calculator to validate — it rejects illegal attachment points.

> **Verified 2026-10-14:** `D` is a legal residue, square brackets may attach to diamino K, and `gm-AEK[D]A` is correct. Theoretical masses: neutral 1010.4292, [M+H]+ 1011.4364; reduced form `gm(Red)-AEK[D]A` 1012.4448 / 1013.4521 ([M+2H]2+ 507.2297); partially amidated variant `gm-AEK[D(Am)]A` 1009.4451 / 1010.4524.

## 4. Multimers: `=` joins stem peptides, `~` joins glycan chains

- `gm-AQK[GGGGG]A=gm-AQK[GGGGG]A (4-3)` — cross-linked dimer; **the donor must come first**
- `gm-AEJA~gm-AEJA` — glycan-linked dimer
- `gm-AEJ=gm-AEJA=gm-AEJAA (3-3, 4-3)` — trimer; cross-link types listed in order in the trailing brackets

## Practical workflow

Paste PGLang into the Mass Calculator → monoisotopic masses computed automatically → export CSV straight into the MS Analysis module. The Fragment Generator annotates each fragment in the same PGLang vocabulary (signature ions distinguish 3-3/4-3 cross-link isomers and rule out mass coincidences).
