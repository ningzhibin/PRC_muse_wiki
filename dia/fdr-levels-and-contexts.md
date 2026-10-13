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
