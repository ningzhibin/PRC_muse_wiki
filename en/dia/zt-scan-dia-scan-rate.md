# Where ZT Scan DIA's 1342 Hz comes from: scan-rate definitions are not comparable

SCIEX advertises "up to 1342 Hz" for ZT Scan DIA 3.0 on the ZenoTOF 8600, but this Hz is **not the same physical quantity** as the Orbitrap Astral's 200 Hz or timsTOF PASEF's 300 Hz, and the numbers cannot be compared directly.

## Deconstructing the figure

SCIEX's own wording: with a quadrupole scan speed of up to **4000 Da/s**, the system "can operate at a MS/MS scan rate of up to 1342 Hz" (dependent on the predefined mass range). Do the division:

$$4000\ \text{Da/s} \div 1342\ \text{Hz} \approx 3\ \text{Da}$$

So ZT Scan DIA's "Hz" = **quadrupole scan speed ÷ encoding step width**. The Q1 isolation window slides continuously, and the data stream is afterwards sliced into one "encoded scan" per ~3 Da of quadrupole travel. The number counts **sampling density along the precursor axis**, not independently accumulated spectra per second at the detector.

The other two platforms quote "real spectra" — every MS/MS has its own injection/accumulation time:

- **Orbitrap Astral**: 200 Hz (Astral Zoom 270 Hz), ~4–5 ms injection per spectrum
- **Bruker timsTOF (PASEF)**: up to 300 Hz, with TIMS parallel accumulation keeping the duty cycle high

In ZT Scan, by contrast, the effective time a precursor spends inside the isolation window is only window width ÷ scan speed: for a 2 Da window at maximum speed, about **0.5 ms**.

## What SCIEX's own application notes actually run

| Source (SCIEX tech note) | Q1 width | Q1 scan speed | MS/MS accumulation | Cycle (400–900 Da) |
|---|---|---|---|---|
| Proteomics LFQ (ZT Scan DIA 3.0) | 2 Da | 296 Da/s | 6.8 ms | 1.8 s |
| GPF library generation (ZT Scan DIA 3.0) | 1 Da | 101 Da/s | 9.9 ms | ~1.1 s |
| Headline maximum | — | **4000 Da/s** | — | — |

Real methods run at 1/13 to 1/40 of the maximum scan speed, with an effective spectral rate on the order of ~150 Hz. For reference, DDA on the same instrument does ~100 Hz MS/MS (<10 ms per spectrum).

## Why it still works

- **Zeno trap**: fragment ions are accumulated in the trap and pulsed into the TOF, pushing the duty cycle above 90%; SCIEX claims 5–20× MS/MS sensitivity — sensitivity compensates for the short accumulation
- **Sliding-window linking**: fragments can be linked back post-acquisition to near-unit-resolution precursor bins, giving better selectivity and less chimeric spectra than wide-window DIA

The approach itself is sound; its real operating point simply sits where everyone else's does: **narrow-window DIA with a ~1–2 s cycle**.

## How to compare "fast" across platforms

Don't compare headline Hz. Compare these three quantities instead:

1. **Cycle time** (how long one pass over the precursor range takes — decides points per chromatographic peak)
2. **Isolation window width** (decides spectral chimericity and selectivity)
3. **Effective accumulation time per spectrum** (decides ion statistics and the low-abundance floor)

## References

- SCIEX ZenoTOF 8600 product page ("ZT Scan DIA 3.0 supports scan speeds up to 1342 Hz"): https://sciex.com/products/mass-spectrometers/qtof-systems/8600-system
- SCIEX ZenoTOF 7600+ product page (the 4000 Da/s → 1342 Hz derivation, verbatim): https://sciex.com/products/mass-spectrometers/qtof-systems/7600-plus-system
- SCIEX tech note: Narrow-window scanning DIA for unbiased label-free quantitation (actual ZT Scan DIA 3.0 parameters, PEAKS processing): https://sciex.com/content/dam/SCIEX/pdf/tech-notes/life-science-research/MKT-38229-A-TN_LFQ_Proteomics_Using_Narrow-Window_DIA_20260525-PEAKS.pdf
- SCIEX tech note: Unlocking deeper DIA performance with empirical spectral libraries generated using ZT Scan DIA 3.0 (GPF library parameters): https://sciex.com/tech-notes/life-science-research/proteomics/unlocking-deeper-dia-performance-with-empirical-spectral-libraries-generated-using-zt-scan-dia-3-0-on-the-zenotof-8600-system
