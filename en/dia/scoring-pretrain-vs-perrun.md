# Scoring-Model Training Strategies in DIA: Global Pretraining vs Per-Run Learning

## Background

In DIA identification, the classifier that scores peak groups follows two training philosophies: DIA-NN's "train a fresh model for each file" and DIA-BERT's "pretrain at scale, then fine-tune per file." This is the same statistical question as the run-specific vs global FDR debate.

## DIA-NN: train per run

- For each MS file, train a shallow neural-network ensemble on that file's own target–decoy data to score peak groups.
- Inputs are a small set of hand-engineered peak-group features — which is also why it runs on CPUs, at the cost of information loss.
- Characteristics: low bias, high variance. No domain-shift problem (it only trusts the file in front of it), but the limited target/decoy sample per file caps the classifier's ceiling, especially for low-abundance precursors.

## DIA-BERT: pretrain + per-file fine-tuning

- Pretrain a transformer scoring model on 952 Orbitrap DIA files first (repo: `resource/model/base.ckpt`).
- When a new file comes in, fine-tune from `finetune_model.ckpt`; quantification uses a separate `quant.ckpt` to estimate peak area.
- Essentially shrinkage / transfer learning: learn the cross-file shared structure first (peak shape, fragment co-elution, isotope patterns and other physicochemical regularities).

## The FDR analogy

- Run-specific FDR: locally well calibrated, but the overall error rate inflates when unioning multiple files.
- Global FDR: controls experiment-wide error, but requires the "global standard" to be valid for every file.
- Scoring models are analogous: **pretraining learns universal regularities; fine-tuning aligns them to the current file**. Neither can be skipped. Nobody dares use a purely global model (DIA-BERT itself fine-tunes), and purely per-run training wastes the shared information across files.

## When each is better

- Routine Orbitrap data: pretraining + fine-tuning wins (DIA-BERT's own benchmarks: +22% precursors, +51% proteins, conservatively lower FDR).
- Out-of-distribution data (timsTOF, Astral and other types not covered by pretraining): the per-run strategy is more robust — for data the model has never seen, learning from scratch beats arriving with a bias.
- Caveats: DIA-BERT's benchmark is self-reported and independent validation hasn't caught up; pretraining bias is implicit (peptide types systematically missing from the training set can never be learned); GPU required (40GB VRAM recommended).

## References

- Liu Z et al. DIA-BERT: pre-trained end-to-end transformer models for enhanced DIA proteomics data analysis. *Nat Commun* 16:3530 (2025). https://doi.org/10.1038/s41467-025-58866-4
- Demichev V et al. DIA-NN: neural networks and interference correction enable deep proteome coverage in high throughput. *Nat Methods* 17:41–44 (2020). https://doi.org/10.1038/s41592-019-0638-x
