# MSFragger and Sage: DDA search speed, output formats, and how to choose

**Compiled**: 2026-11-05, from a Q&A discussion about MSFragger earlier that day. The trigger was the Paulo & Shuken 2026 JPR paper comparing DIA/DDA on an Orbitrap Ascend, whose DDA condition used MSFragger-DDA+. This is not a deep read of a single paper; it collects the questions from that discussion: what DDA+ is, how fast MSFragger is, what its outputs look like, how Sage compares, and whether to run MSFragger inside FragPipe or standalone.

## 1. MSFragger-DDA+: recovering co-fragmented peptides from chimeric spectra

Traditional DDA search assumes one MS2 spectrum corresponds to one peptide — the precursor the instrument selected — and only searches candidates within a narrow 5–20 ppm window around it. In practice, the isolation window (typically 0.7–2 Th) often admits several peptides at once, which fragment together, producing chimeric spectra. Those co-fragmented peptides are simply never searched for, so their fragments are wasted.

The problem has been known for a long time. The older fix was to detect MS1 features or deconvolve the spectrum first, then search — but that requires the co-fragmented peptide to have a high-quality precursor peak in MS1, and low-abundance peptides often have weak or undetectable MS1 peaks, so they are missed. DDA+ reverses the order:

1. **Search first, no MS1 preprocessing.** MSFragger's fragment-ion indexing is fast enough to search every possible precursor inside the full isolation window.
2. **Then verify peaks and rescore.** Targeted XIC extraction for each match (the theoretical m/z and charge are known by then), isotope-distribution comparison by Kullback–Leibler divergence to discard poor peak shapes, then a greedy algorithm that removes fragments shared by multiple peptides and rescores.

Reversing the order recovers low-abundance co-fragmented peptides while keeping FDR under control. Reported gains (Yu, Deng & Nesvizhskii, *Nat Commun* 2025;16:3329):

- On average **57% more peptide sequences** than conventional MSFragger DDA, ranging from 15% (AspN/GluC + CID) to 45% (trypsin + HCD) depending on enzyme and fragmentation;
- 16–19% more proteins quantified without MBR, 11–13% more with MBR;
- Smaller gains for phosphoproteomics: ~5% more sites, ~14% more phosphosequences.

Another distinctive feature: native support for **WWA (wide-window acquisition)** — the large-isolation-window DDA used in single-cell proteomics, whose spectra are as multiplexed as DIA. Conventional DDA tools cannot handle them, and DIA tools cannot either (no XIC extraction possible). DDA+ is one of the few search modes that handles such data directly.

Status: built into MSFragger since version 4.0 (December 2023); paper published April 2025. Paulo & Shuken used DDA+ as their DDA condition; their Fig. S6 shows it identifies more than standard DDA, and the authors note its advantage would be even larger without FAIMS (more chimeric spectra). So the DIA–DDA gap in that paper was already measured against a "strengthened" DDA — with standard DDA search, the gap would only be wider.

## 2. How fast is MSFragger?

The core trick is fragment-ion indexing: all theoretical fragment ions from the database are built into an index once, and each spectrum is matched by index lookup instead of scoring candidate peptides one by one. The original Kong et al. 2017 *Nat Methods* paper reports it as more than 100-fold faster than most existing tools.

Concrete numbers (open search, one LC-MS/MS run, quad-core workstation): Comet 13.6 hours, X!Tandem 16.3 hours, MSFragger 5.4 minutes; Tide 176.7 minutes vs MSFragger 9.8 minutes under the same constraints; SEQUEST HT over 11 hours on an eight-core machine.

In practice: a routine closed search of a 60–90 min DDA run finishes in minutes on an ordinary desktop; an open search (±500 Da for unknown modifications) that used to run overnight is now also minutes. The DDA+ paper notes a tryptic search runs on a standard desktop with 32 GB RAM and a mainstream CPU, with 64–128 GB recommended for phosphopeptide or non-specific digestion searches.

Two caveats: speed depends on database size, number of variable modifications, and search window, and building the peptide index itself takes a few minutes the first time; downstream MSBooster deep-learning rescoring and Percolator add time (MSBooster is faster with a GPU, but runs on CPU too). Compared with MaxQuant (Andromeda), MSFragger is typically an order of magnitude faster on the same data.

## 3. What do the outputs look like?

Two layers: the engine itself, and the full FragPipe pipeline.

**The MSFragger engine itself** writes one pepXML per spectral file (`<run>.pepXML`) by default; open searches and mass-offset searches additionally produce a TSV (`<run>.tsv`); for other modes, set `output_format = tsv_pepXML` in fragger.params to get the TSV.

**The full FragPipe pipeline** (MSFragger → MSBooster → Percolator → Philosopher → IonQuant) produces TSV reports:

- `psm.tsv`: FDR-filtered PSMs, one row per spectrum match;
- `ion.tsv`: ion level (charge-state-specific), with quantification;
- `peptide.tsv`: peptide level;
- `protein.tsv`: protein level (`combined_protein.tsv` for multiple experiments or plexes).

Intermediate files include `[filename].pin` (Percolator input), `interact-[filename].pep.xml`, `combined.prot.xml` (protXML protein inference), `protein.fas` (FDR-filtered protein sequences), and `fragger.params` (search parameters, for reproducibility). TMT analyses add a `tmt-reports/` folder with abundances and ratios at gene/protein/peptide/modification-site levels; PTM-Shepherd results go in `ptm-shepherd-output/`. For DIA via MSFragger-DIA + DIA-NN, the final output is DIA-NN's `report.tsv`/`report.parquet`.

pepXML is a standard format; convert it to other formats (e.g. mzIdentML) with ProteoWizard's idconvert. Note that MSFragger's pepXML carries some attributes beyond the official schema (e.g. `ion_mobility`), which some older tools may trip over.

## 4. Sage vs MSFragger

**What Sage is.** A search engine written in Rust by Michael Lazear (*J Proteome Res* 2023;22:3652–3659), a clean-room reimplementation of MSFragger's fragment-ion indexing idea, with Rust's execution efficiency pushing speed further. A single static binary, MIT-licensed, command-line only, mzML input only, with built-in LFQ and TMT quantification, retention-time prediction, and FDR control, designed for cloud and large-scale queues.

**Speed: Sage is faster.** The Sage paper's abstract says identification performance matches the best existing tools while running an order of magnitude faster; the author's blog gives specifics for a typical TMT search: ~5× faster than MSFragger, 20× faster than Comet, 500× faster than MSGF+, with similar PSM counts at 1% FDR. Third parties agree: rustims (2025) calls Sage "an order of magnitude faster than the already rapid MSFragger", and Dear-PSM (2024) measured Sage using ~5× less memory than MSFragger. Note the Sage paper's benchmark was done by its own author — treat the numbers with a discount.

**Identification counts: broadly comparable, with an FDR pitfall.** On routine DDA the two are close: in the FastaLake metaproteomics work, peptide counts in healthy stool samples had a median ratio of 1.00. But the MS Andrea paper (2026, open search) showed that with standard target-decoy FDR, Sage reported only 24 PSMs on average on a histone dataset, vs 418 for MSFragger + PeptideProphet; using Sage's own spectrum-level q-values, that jumped to 677; with model-based post-processing applied uniformly, all three engines were comparable again. In other words, **how FDR is computed hugely affects a Sage comparison** — comparing raw numbers directly misleads.

**MSFragger wins in specific scenarios.** In a citrullination dilution series (*Nat Commun* 2026), MSFragger reported more PSMs than MaxQuant and Sage at every input level, because its labile/hybrid mode captures diagnostic ions from neutral loss. In immunopeptidomics (*JPR* 2025), four engines (PEAKS, Comet, Sage, MSFragger) agreed strongly, and what actually separated results was MS2Rescore rescoring (+27% identifications on average).

**Feature gap: Sage is a lean single tool, MSFragger is an ecosystem.** Sage's open search supports only a limited number of variable modifications, has no multi-modification combination scoring, and no PTM-Shepherd-style modification profiling. MSFragger + FragPipe offers MSBooster deep-learning rescoring, Percolator, IonQuant, DDA+, DIA, glycopeptides, and labile mode.

**How to choose.** Search speed is a solved problem for both — one DDA run takes minutes either way, so that is not the bottleneck. For routine DDA service samples, Sage's single binary drops into a script easily and suits automation; for TMT, phosphoproteomics, open searches for unknown modifications, or when a GUI and complete reports are wanted, FragPipe + MSFragger is the smoother choice. Neither replaces DIA-NN for DIA. If you try Sage, two things to remember: input is mzML only (.raw must go through MSConvert first), and use its own q-values for FDR rather than comparing its standard target-decoy counts against MSFragger.

## 5. FragPipe or standalone MSFragger?

Short answer: FragPipe in most cases. MSFragger is just the search engine; FragPipe is the whole pipeline.

**Limitations of standalone MSFragger** — you get only the first step of the pipeline and must connect the rest yourself:

- No FDR control: the pepXML is unfiltered; you must run PeptideProphet (TPP) or Philosopher yourself, and Percolator needs .pin files first;
- No protein inference: no protXML, no protein groups;
- No quantification: LFQ needs IonQuant, TMT needs TMT-Integrator, SILAC likewise;
- No MBR, no MSBooster rescoring, no Crystal-C mass calibration, no PTMProphet localization, no PTM-Shepherd, no EasyPQP library building;
- Parameters are hand-edited fragger.params text files, with no presets or validation — misconfiguration is easy to miss. In FragPipe, DDA+ is just a file-type dropdown; standalone, you need to know the right parameters;
- Tool downloads and updates are manual.

A third option, the Proteome Discoverer node, has limited functionality.

**FragPipe is not GUI-only.** Since version 18.0 it has a headless mode:

```
fragpipe --headless --workflow <workflow file> --manifest <manifest file> --workdir <results directory>
```

Workflow and manifest files are typically configured and exported once in the GUI, after which batch runs can go on a server or HPC; there are also official Docker/Singularity images, and Frag'n'Flow (a Nextflow wrapper) for large-scale deployment. In other words, the idea of "hand raw files to a tool and let it run the search automatically" is essentially what FragPipe headless plus a saved workflow already provides, without building anything from scratch.

**Standalone MSFragger makes sense when**: you only need the search step and process pepXML/TSV with your own code; you embed it in another pipeline (Galaxy, Nextflow, a custom automation pipeline); or you only want the engine itself on a cluster, without the GUI dependencies.

**Practical advice for the PRC**: one-off service samples go through the FragPipe GUI (the LFQ-MBR workflow does search + FDR + protein inference + quantification in one go); automation goes through headless mode. Run MSFragger standalone only when you explicitly want raw PSMs and can handle the downstream steps yourself.

**Licensing note**: MSFragger is free for academic, non-commercial, and educational use, but commercial licensing has been handled by Fragmatics, LLC since 1 May 2025. Whether a core facility's paid external service counts as "non-commercial" is worth confirming before use.

## References

- Kong AT et al. MSFragger: ultrafast and comprehensive peptide identification in mass spectrometry-based proteomics. *Nat Methods*. 2017;14(5):513–520. doi:10.1038/nmeth.4256.
- Yu F, Deng Y, Nesvizhskii AI. MSFragger-DDA+ enhances peptide identification sensitivity with full isolation window search. *Nat Commun*. 2025;16:3329. doi:10.1038/s41467-025-58728-z.
- Lazear MR. Sage: an open-source tool for fast proteomics searching and quantification at scale. *J Proteome Res*. 2023;22(11):3652–3659. doi:10.1021/acs.jproteome.3c00486.
- Paulo JA, Shuken SR. Evaluating data acquisition modes and fragmentation types in label-free proteomics on an Orbitrap Ascend Tribrid mass spectrometer. *J Proteome Res*. 2026. doi:10.1021/acs.jproteome.6c00748. (Trigger of the discussion; its DDA condition was MSFragger-DDA+.)
- MSFragger wiki: Interpreting MSFragger Output. https://github.com/Nesvilab/MSFragger/wiki/Interpreting-MSFragger-Output
- FragPipe documentation: Guide to FragPipe results; Running FragPipe in command line interface (headless mode). https://fragpipe.nesvilab.org/
