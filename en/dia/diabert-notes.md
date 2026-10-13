# DIA-BERT Notes

## Paper

Liu Z, Liu P, Sun Y, et al. DIA-BERT: pre-trained end-to-end transformer models for enhanced DIA proteomics data analysis. *Nat Commun* 16:3530 (2025).
https://doi.org/10.1038/s41467-025-58866-4

Code: https://github.com/guomics-lab/DIA-BERT (free for academic use, no commercial use)

## One sentence

An end-to-end transformer for DIA identification + quantification — not a spectral library generation tool.

## Architecture highlights

- Scoring model pretrained on 952 Orbitrap DIA files
- File-specific fine-tuning for each new file
- Quantification via a separate transformer estimating peak area; training used some simulated DIA data

## Model files in the repo (`resource/model/`)

| File | Size | Purpose |
|---|---|---|
| `base.ckpt` | ~12 MB | Pretrained scoring model; loaded at identification time to score peak groups |
| `finetune_model.ckpt` | ~12 MB | Starting weights for fine-tuning; fine-tuning each file starts from it |
| `quant.ckpt` | ~11 MB | Quantification model; estimates peak area |

All in PyTorch Lightning checkpoint format.

## Practical limitations

- GPU required (40GB VRAM recommended), 40GB+ RAM, 100GB disk
- v1.0 trained on Orbitrap data; timsTOF / tripleTOF not recommended by the authors
- PTMs and non-tryptic peptides not validated
- Astral data "not thoroughly evaluated" — runs with instrument type "Other" but use with caution
- No direct spectral library generation from FASTA (generate a tsv library with DIA-NN and import it)

## Libraries used in benchmarks

The library-based benchmark used [DPHL v2](../libraries/dphl-v2.md) (RF version, 601,982 precursors).
