# DIA 中 FDR 控制的两个维度：层次与语境

"1% FDR" 不说清**层次**和**语境**是没有意义的。

## 两个维度

- **层次（level）**：precursor → peptide → protein group。在 precursor 层面控制 1% FDR，不等于蛋白层面 1%——肽段错误会向蛋白层放大（错误肽段随机散落成假蛋白，正确肽段聚向少数真蛋白）。
- **语境（context）**：run-specific vs experiment-wide vs global（Rosenberger 等，2017 年系统论述）。

## 为什么不能每个文件单独卡 1% 再合并

每个文件单独按 1% FDR 过滤、再取多文件并集：真阳性在各文件间高度重叠，假阳性各不相同，并集的 protein FDR 会远高于 1%，文件越多膨胀越严重。

## 实践：看哪一列

- 看单个文件：用 run-specific q-value（DIA-NN 的 `Q.Value` / `PG.Q.Value`）
- 做跨文件矩阵、差异分析：用 global q-value（DIA-NN 的 `Global.Q.Value` / `Global.PG.Q.Value`，即对整个实验计算）
- DIA-NN 的 matrix 文件按 1% FDR 过滤：蛋白/基因矩阵用 global q-value，precursor 矩阵用 global + run-specific q-value；`--matrix-spec-q` 可再强制加一层 run-specific protein group 过滤。所以 report 和 matrix 的蛋白数对不上是**正常的**，不是 bug。

## 和打分策略的呼应

- per-run 打分（DIA-NN）：每个 run 用自己的 decoy 分布训练和校准，天然适配 run-specific 语境
- 预训练模型（DIA-BERT）：分数需要逐文件 fine-tune 对齐，FDR 估计同样要选对语境

相关：[DIA 打分模型的训练策略：全局预训练 vs 逐文件学习](scoring-pretrain-vs-perrun.md)

## 参考

- Rosenberger et al., 2017（run-specific / experiment-wide / global 三层语境的系统论述）
- DIA-NN 官方文档：Global q-value 定义与 matrix 过滤规则

---

# English

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
