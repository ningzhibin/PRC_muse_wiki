# Commercial RP magnetic beads: options for secretome enrichment + on-bead digestion

**Compiled**: 2026-11-04, from that day's secretome-enrichment discussion. Prices and availability follow the vendors' current catalogs; only datasheet information verifiable at the time is included here — this is not an exhaustive search.

## Why RP magnetic beads

Secretome samples mean little protein, large volumes, and culture medium as the matrix. RP magnetic beads bind through hydrophobic interactions, which gives them two properties that suit this scenario: first, they tolerate salt (the ~150 mM salt in culture medium does not interfere with binding and can even promote it through salting-out), unlike ion-exchange beads that need low-salt conditions; second, binding is essentially pH-independent, so proteins stay bound to the beads during digestion at pH 8 — genuine on-bead digestion. That differs from SCX (released at pH 8) and SAX (only the acidic subset stays bound at pH 8); see the other note in this section, [Digestion in the tip or on the tip](sample-prep/in-tip-digestion-geometries.md).

## Products

**MagSiproteomics C4 / C8 / C18 (Magtivio).** Magnetic silica beads with C4, C8, or C18 alkyl chains, supplied at 10 mg/mL in 25% ethanol in 2 mL and 10 mL sizes. The datasheet assigns distinct roles: C4 for capture, concentration, and purification of proteins from protein mixtures; C18 for peptide desalting and concentration (explicitly including "secreted peptides into media") and for peptide purification from serum and plasma samples.

**BcMag C-18 (Bioclone).** Approximately 1 µm silica-coated iron oxide superparamagnetic beads with C18 alkyl groups. Binding capacity >20 µg protein/mg of beads; the datasheet recommends 0.5 mg of beads for about 10 µg of protein. Selection guidance: C18 for small proteins/peptides, C8 for low-to-intermediate molecular weight proteins, C4 for larger molecular weight proteins. Binding conditions are 0.1–1% TFA at pH < 4; elution uses 70% acetonitrile.

**Dynabeads RPC 18 / RPC Protein (Thermo).** 1 µm monodisperse magnetic beads. RPC 18 targets peptides <5,000 Da for concentration and desalting; RPC Protein targets proteins and large peptides >5,000 Da; each mg of beads adsorbs approximately 14 µg (measured with a peptide standard). The same historical product line also included WCX (weak cation exchange) and SAX (strong anion exchange) magnetic beads — so SAX magnetic beads are commercial products too. Note: this is an older product line; confirm with Thermo that it is still sold before ordering.

## Selection notes

- **Chain length**: C18 binds most tightly — it captures well but intact large proteins are hard to elute. That does not matter for on-bead digestion, since what is eluted at the end are peptides (which come off easily in 50–70% acetonitrile). If intact proteins must be eluted before digestion, choose C4.
- **Capacity math**: at BcMag's >20 µg/mg, even 10 µg of secreted protein in 10 mL of medium theoretically needs only 0.5–1 mg of beads — bead amount is not the bottleneck.
- **Binding conditions**: RP binding generally needs acidic conditions (0.1% TFA, pH < 4); culture medium can simply be acidified. Organic solvents in the sample must be removed first, and detergents such as SDS diluted below the datasheet limits.
- **The real unknown**: most datasheets position these beads for peptide desalting or small-volume concentration, not for "capturing intact secreted proteins from milliliter volumes of medium". Binding kinetics from dilute solutions (where proteins reach the bead surface by diffusion, so incubation time matters) must be verified in a pilot experiment.
- **Comparison with SAX**: SAX magnetic beads carry a pI bias (they only bind proteins with pI below the buffer pH, so basic secreted proteins are lost in the flow-through) and suffer salt competition from culture medium. RP has neither problem, but its cost is slow on-bead digestion (diffusion-limited, as with in-disk C18 digestion — overnight is the norm).

## Product links

- [MagSiproteomics C4 / C8 / C18 datasheet (Magtivio, via AMSBIO)](https://resources.amsbio.com/Datasheets/MD01009.pdf)
- [BcMag C-18 Magnetic Beads datasheet (Bioclone)](https://www.bioclone.us/files/BcMag_C-18_Magnetic_Beads.pdf)
- [Magnetic Bead Chromatography brochure (Thermo, covering Dynabeads RPC 18 / RPC Protein / WCX / SAX)](https://assets.thermofisher.com/TFS-Assets/LSG/brochures/Magnetic_Bead_Chromatography.pdf)
