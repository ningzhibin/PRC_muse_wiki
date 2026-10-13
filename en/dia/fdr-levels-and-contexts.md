# Two Dimensions of FDR Control in DIA: Level and Context

"1% FDR" is meaningless without specifying the **level** and the **context**.

## The two dimensions

- **Level**: precursor → peptide → protein group. Controlling 1% FDR at the precursor level is not the same as 1% at the protein level — peptide errors amplify upward to the protein layer (false peptides scatter randomly into false proteins, while true peptides concentrate into a small number of true proteins).
- **Context**: run-specific vs experiment-wide vs global (systematically discussed by Rosenberger et al., 2017).

## Why you can't filter each file at 1% and then merge

Filtering each file separately at 1% FDR and then taking the union across files: true positives overlap heavily between files while false positives differ — the union's protein FDR will be far above 1%, and the inflation worsens with more files.

## Practice: which column to look at

- Looking at a single file: use run-specific q-values (DIA-NN's `Q.Value` / `PG.Q.Value`).
- Building cross-file matrices or differential analysis: use global q-values (DIA-NN's `Global.Q.Value` / `Global.PG.Q.Value`, computed across the whole experiment).
- DIA-NN matrix files filtered at 1% FDR: protein/gene matrices use global q-values; precursor matrices use global + run-specific q-values; `--matrix-spec-q` can additionally force a run-specific protein-group filter. So protein counts differing between report and matrix files is **normal**, not a bug.

## Echoing the scoring strategy

- Per-run scoring (DIA-NN): each run trains and calibrates on its own decoy distribution, naturally fitting the run-specific context.
- Pretrained models (DIA-BERT): scores need per-file fine-tuning to align, and FDR estimation likewise requires the right context.

Related: [Scoring-model training strategies in DIA: global pretraining vs per-run learning](scoring-pretrain-vs-perrun.md)

## References

- Rosenberger et al., 2017 (systematic treatment of the run-specific / experiment-wide / global three-layer context)
- DIA-NN documentation: global q-value definition and matrix filtering rules
