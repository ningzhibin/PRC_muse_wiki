# Digestion in the tip or on the tip: where six solid-phase digestion geometries put the protein

**Compiled**: 2026-11-04, from that day's discussions of Evotip structure, in-StageTip, and SISPROT. There is one core question: when digestion happens, where exactly is the protein — in solution, on a solid-phase surface, inside a solid-phase disk, or inside a porous matrix? The location decides whether the protease can reach the protein, how long digestion takes, and where the peptides end up.

## Six geometries at a glance

- **One-Tip / Evotip: liquid phase, above the disk** — the digestion liquid sits on top of the C18 disk without soaking into the bed; the C18 only becomes a trap at the final step.
- **in-StageTip / C18: entrapped inside the disk** — protein is trapped inside the C18 disk with a wide distribution; digestion happens in the disk and takes overnight.
- **AnExSP / AX: on the disk surface** — protein localizes in a layer on the AX disk surface without penetrating it; surface enrichment gives the protease easy access, and digestion completes in 1–4 hours.
- **SISPROT / SCX: three stages linked by a pH switch** — capture at low pH, release and liquid-phase digestion at high pH, transfer to C18 at pH 10.
- **SP3 / PAC: digestion while bead-bound** — protein stays bound to magnetic beads throughout digestion.
- **S-Trap: digestion inside a matrix** — protein is trapped in the pores of a fiber/membrane matrix, and digestion happens inside the matrix.

## One-Tip: the digestion liquid sits on top of the disk

The bottom of an Evotip is a small composite C18 disk: resin particles immobilized in a porous matrix, functionally similar to an Empore disk. Evosep's patent (US12298284B2) describes the resin as "embedded and fixated therein", and the word "frit" never appears in the full text — there is no frit-below-plus-cover-frit sandwich; the disk is held in place by the tip's taper.

The 2024 cell version (Nat Commun s41467-024-46777-9) shows the "liquid-phase digestion above the disk" most clearly:

- After adding 5 µL of lysis/digestion buffer + 5 µL of cell suspension, only a brief **50 g spin** is used, to bring the liquid into contact with the C18 surface and remove air bubbles — not to push it through;
- **800 g × 60 s is the push-through setting**, used for preconditioning (20 µL Solvent B rinse, 20 µL Solvent A equilibration) and for post-digestion loading (50 µL Solvent A);
- During incubation, water is added to the Evotip box up to the level of the C18 resin, so the outlet is submerged, with no pressure differential and no evaporation, and the liquid sits stably on the disk;
- Spinning at 800 g right after adding digestion buffer is an operating error for One-Tip: the reaction volume is pushed through prematurely and binding becomes uneven.

In the plasma version (Logerot et al., Anal Chem 2026), digestion also happens in the liquid above the disk: 10 µL of 0.2% DDM / 100 mM TEAB / 10 mM TCEP at 37 °C for 30 min for denaturation, 2 µL IAA (final 44 mM) for 30 min at room temperature in the dark for alkylation, 3 µL trypsin at an enzyme:substrate ratio of 1:14 at 37 °C for 4 h for digestion; then 50 µL of 0.1% FA is added and the peptides are loaded onto the C18 at 800 g for 60 s. Note that Evotips do not tolerate temperatures above 37 °C, which is one reason the plasma version does not use higher temperatures.

## in-StageTip: protein is digested inside the C18 disk

Kulak et al. (Nat Methods 2014, Mann group) put the entire workflow, from cell lysis to elution of purified peptides, inside a single tip fitted with a C18 Empore disk. Where the protein sits in the disk was verified directly by Takemori et al. in 2022 using fluorescent b-phycoerythrin: **in C18 RP StageTips, the protein was entrapped inside the disc and showed a wide distribution**.

The consequence is time: every protocol derived from this method digests overnight — the Wageningen modified version incubates trypsin solution with shaking at room temperature overnight, and another applied paper uses LysC + trypsin at 37 °C overnight. Endpoint depth is good (Kulak: quadruplicate measurements of a human cell line, copy-number estimates for 9,667 proteins, R² = 0.97 between replicates), but the bottleneck is protease diffusion into the disk. Kulak's paper published no dedicated digestion-kinetics metric (no substrate-disappearance time course, no missed-cleavage rate), so do not infer one retroactively.

## AnExSP: protein is digested on the AX disk surface

Takemori et al. (Chem Commun 2022) switched to a single AX (anion exchange) disk because the C18 system cannot tolerate SDS and Coomassie Brilliant Blue (CBB). The other half of their localization experiment: **on the AX disk, the protein was localized on the disc surface in a layered manner and did not penetrate into the disc**, and the authors concluded that surface enrichment "facilitates access for proteases".

Digestion kinetics were measured only on the AX disk, and the numbers are clean: for 10 µg of human cellular protein extract with Trypsin/Lys-C at a substrate:enzyme ratio of 20:1, most substrate disappeared within 30 minutes and digestion was complete in 1–4 hours (25 °C); at 100:1, digestion was incomplete even after 4 hours; DIA results gave about 7,000 proteins from a 4-hour digestion, identical to an overnight 18-hour digestion. For SDS handling, SDS is trapped on the disk together with the protein, and peptides are eluted with 0.5% formic acid / 30% acetonitrile without SDS coming along; but when the SDS concentration is too high, protein penetrates into the disk, and the authors state explicitly that this "mak[es] digestion on the disc difficult" — direct evidence that penetration means poor digestion, and a contrast that highlights the disadvantage of the C18 in-disk distribution.

A naming note: this paper uses AX throughout (its SI specifies a 3M Empore AX SPE disc) and never uses the term SAX; nor did it run a head-to-head digestion-efficiency comparison of AX versus C18, so do not cite it as "SAX digests better than RP".

## SISPROT: SCX capture, liquid-phase digestion, C18 peptide capture

SISPROT (Chen et al., Anal Chem 2016, Tian group) packs SCX beads on top and a C18 disk below in one tip. The pH sequence published by Lin et al. in 2018 is explicit:

- **pH 2–3 loading**: the sample is acidified with 0.1% formic acid to pH 2–3 before loading, so proteins carry positive charge and bind the SCX beads;
- **pH 3 washing and pH 3 reduction**: wash with 20% acetonitrile / 8 mM potassium citrate (pH 3), reduce with 10 mM TCEP / 9 mM potassium citrate (pH 3) for 15 minutes — reduction is deliberately done at pH 3 so proteins do not come off early;
- **pH 8 digestion**: trypsin in 100 mM Tris-HCl (pH 8, containing 10 mM IAA, so alkylation and digestion run together) is injected into the tip, 1 hour at room temperature;
- **pH 10 transfer**: 200 mM ammonium formate (pH 10) transfers the peptides from SCX to the C18 disk below, followed by elution.

The key correction: **SISPROT digestion is not surface digestion in the bound state**. At pH 8, proteins are released from the SCX, and digestion happens in the liquid phase inside the tip; the predecessor proteomic-reactor literature states explicitly that at pH 8 "the proteins are released from the SCX beads and digested with trypsin". Release is not loss: the liquid never leaves the tip, and after digestion the peptides are actively transferred to C18 at pH 10 as a safety net. The autoSISPROT paper (Wu et al., Chem Sci 2024) phrase "digestion on the beads" is a simplification and should not be read literally. In detail, high-pI proteins still carry positive charge at pH 8 and release more slowly — a gradual "cut-and-release" process — but everything ends up captured on C18.

Two SISPROT-family variants: the original uses SCX beads; 3D-SISPROT (Chen et al., J Chromatogr A 2017) uses SAX membranes + C18 membranes for in-tip digestion and three-dimensional fractionation; autoSISPROT uses mixed SCX/SAX beads.

## SP3 / PAC and S-Trap: two more bound-state / in-matrix digestions

- **SP3**: protein stays bound to carboxylate magnetic beads during digestion; after digestion, the PRC's current practice is the 95% ACN peptide recapture step (see the other note in this section, [SP3 peptide cleanup](sample-prep/sp3-peptide-cleanup.md)). The BAMM multi-omics version switched to unmodified silica beads (Cytiva SeraSil-Mag 700) with Rapid Trypsin digestion at 60 °C for 40 minutes with 1000 rpm shaking throughout — the reason for unmodified beads is that carboxylate beads capture polar metabolites, not better protein binding.
- **S-Trap**: protein is trapped in the pores of a quartz fiber/membrane matrix, and digestion happens inside the matrix; in the One-Tip plasma paper, the S-Trap comparator was run at 37 °C overnight precisely because Evotips cannot tolerate Protifi's standard 47 °C / 1 h condition.

## Rules of thumb: ask where digestion happens, not just what it happens on

- When a method says "on-bead / on-disc digestion", ask two questions first: is the protein still bound to the solid phase during digestion? And did the digestion buffer's pH change relative to the binding pH? SISPROT's pH 8 release is the counterexample: literally "digestion on beads", actually liquid-phase digestion.
- Accessibility sets the time (the compiler's judgment, not a literature quote): liquid phase (One-Tip, 4 h) and disk surface (AX, 1–4 h) are fastest; bead-bound digestion (SP3) sits in the middle; in-disk trapping (C18 in-StageTip) and in-matrix digestion (S-Trap) depend most on diffusion or long incubations. In practice, the time budget and whether overnight is acceptable often decide the choice before absolute identification numbers do.
- The solid phase can play different roles at different steps in the same tip: in SISPROT, SCX handles capture and C18 handles peptide desalting; in One-Tip, the C18 is just the vessel floor during digestion and only becomes a trap column afterwards. Conflating "reactor" with "trap column" is the most common misreading of this family of methods.

## Sources

- Kulak NA, Pichler G, Paron I, Nagaraj N, Mann M. Minimal, encapsulated proteomic-sample processing applied to copy-number estimation in eukaryotic cells. *Nat Methods* 2014;11:319–324. DOI 10.1038/nmeth.2834.
- Takemori A, Kawashima Y, Takemori N. Bottom-up/cross-linking mass spectrometry via simplified sample processing on anion-exchange solid-phase extraction spin column. *Chem Commun* 2022;58:775–778. DOI 10.1039/d1cc05529a.
- Chen W, Wang S, Adhikari S, Deng Z, Wang L, Chen L, Ke M, Yang P, Tian R. Simple and integrated spintip-based technology applied for deep proteome profiling. *Anal Chem* 2016;88:4864–4871. DOI 10.1021/acs.analchem.6b00631.
- Lin L et al. Fast quantitative urinary proteomic profiling workflow for biomarker discovery in kidney cancer. *Clin Proteomics* 2018;15:42 (source of the SISPROT protocol details).
- Chen W et al. 3D-SISPROT. *J Chromatogr A* 2017;1498:207–214. DOI 10.1016/j.chroma.2017.01.033.
- Wu Q et al. High-throughput drug target discovery using a fully automated proteomics sample preparation platform (autoSISPROT). *Chem Sci* 2024;15:2833–2847. DOI 10.1039/D3SC05937E.
- Logerot E et al. One-Tip plasma proteomics. *Anal Chem* 2026. DOI 10.1021/acs.analchem.6c04223 (plasma version); 2024 cell version, *Nat Commun* s41467-024-46777-9.
- Evosep ApS patent US12298284B2 (source of the Evotip resin-immobilized, no-frit wording); Kverneland et al. *Mol Cell Proteomics* 2024;23:100790 (source of the "EvoTip as a specialized StageTip" wording).

## Still unverified

- Kulak's original digestion details: the full text is paywalled; only the abstract and references were read. Digestion conditions cited here come from the Wageningen modified protocol and an applied paper's supplement, not from Kulak's own Methods.
- The matrix material embedding the resin in Evotip's composite disk: not disclosed by Evosep.
- How gradual the "cut-and-release" of high-pI proteins is in SISPROT at pH 8: not quantified; only the mechanism's direction is stated here.
