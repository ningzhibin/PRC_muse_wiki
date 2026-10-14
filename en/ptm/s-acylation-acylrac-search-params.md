# S-acylation enrichment (Acyl-RAC/ABE) needs no special search parameters

## In one sentence

The enrichment chemistry (hydroxylamine cleavage of the thioester bond) already removes the palmitoyl group — the peptides entering the mass spectrometer don't carry the modification, so standard search parameters apply. No palmitoylation variable modification needed.

## Why: the modification is removed during enrichment

The standard Acyl-RAC / acyl-biotin exchange workflow:

1. Block free thiols (e.g., NEM)
2. **Hydroxylamine (HA) cleaves the thioester bond** — the palmitate is released here
3. Newly exposed thiols are captured on thiopropyl sepharose resin (Acyl-RAC) or biotin-tagged and pulled down (ABE)
4. Reductive elution (e.g., BME/DTT)

So the enriched cysteines are **free thiols** (reduced in the eluate); after reduction + alkylation in SP3 they all become carbamidomethylated — indistinguishable from an ordinary sample.

## Search parameters (consistent across the literature)

Published S-acylome studies all use standard parameters:

- Enzyme: trypsin
- Fixed modification: carbamidomethyl (C)
- Variable modifications: oxidation (M), acetyl (protein N-term)
- **No palmitoylation variable modification**

Sources: the Collins/Woodley site-specific ABE protocol, the PPT1-KO mouse brain Acyl-RAC study (explicitly: "alkylation with iodoacetamide to modify previously palmitoylated cysteines with a carbamidomethyl moiety"), and the neuronal S-acylation Scientific Reports paper.

## The HA− control

HA− = minus-hydroxylamine negative control (no thioester cleavage → S-acylated proteins not enriched). Search it with identical parameters; downstream, compare HA+ vs HA− enrichment (t-test + FDR).

## Elution buffer compatibility

A typical Acyl-RAC eluate (e.g., 1% BME + 0.2% SDS + 250 mM NaCl + HEPES/TX-100) poses no problem for SP3: SP3 is SDS-compatible and BME is removed during bead washes.

## What actually matters

Not modification parameters, but the **species FASTA**: DIA-NN library-free searches run against a species database, and "mammalian cells" doesn't pin down human/mouse/rat — confirm the cell line before setting up the search.

## Related project

Dale Martin (UWaterloo) S-acylation pulldown redo batch 1501WD1366 (Oct 2026): 16 tubes arrived, use only the 8 "PD"-labelled pulldowns, set lysates aside; reuse the previous run's search parameters.
