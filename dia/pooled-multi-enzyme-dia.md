# 多酶混合单发 DIA：一次混合消化物采集 + 联合库一次搜库

## 方案（Richards 配方）

1. 同一份样品分装，分别用 trypsin、AspN、GluC 等互补酶消化（消化完成后淬灭）
2. 消化物按比例混合成一管
3. 一次 DIA 采集（每个样品一个文件）
4. 对 **union library** 搜一次、做一次 FDR 控制

关键：不要把同一个文件 per enzyme 各搜一遍再合并结果（三次搜库 + 三次 FDR 并集合并）。

## 为什么 per-enzyme 搜和合库搜在 DIA 里几乎等价

DIA 搜库是 peptide-centric 的：搜库 = 对库里每个 peptide，从 multiplexed MS2 里提取色谱和碎片证据。酶的身份只在**候选肽集合的定义**这一步起作用；一旦候选肽集合定了，搜库过程不再知道这条肽是哪个酶切出来的。per-enzyme 分开搜（三套候选肽）vs 合库一次搜（同一套候选肽的并集），评价的是同一批肽对同一份数据的证据——结果本质上一样。

真正的差别在 FDR 记账：分开搜每遍各自按 1% 校准，三个 1% 的并集真实 FDR 膨胀到 >1%；合库一次搜，一次 target-decoy 全局控制，报的就是得的就是。合库搜不是"等价但麻烦"，而是**同一个事情的干净实现**。这也是 Richards / Fossati 采用合库的原因。

## 实现细节：union 的准确含义

Union library 必须 = 每个酶**单独**做 in-silico 酶切（或实测 DDA 建库）后，把候选肽集合合并起来的**并集**。

不能是：一次 in-silico 消化把所有酶切位点都堆进 motif 列表。DIA-NN 这类工具把 `--cut` 里列出的 motif 视为"此处皆可切"，会产生**混合末端肽**（一端 tryptic、另一端 GluC）——这种肽在真实混合液里不可能存在（消化物混合前就已淬灭，没有肽见过两个酶），等于凭空放大搜索空间，交不必要的 FDR 税。

分开搜剩下的唯一价值是 per-digest QC 和看每个酶的贡献；这部分可以在合库里给每条肽打一个"来源酶"标签来部分恢复。

## 关键文献

- **Richards AL et al.** J Proteome Res 2022;21(4):1124–1136. DOI 10.1021/acs.jproteome.1c00960. Dataset PXD027242. Trypsin/AspN/GluC 消化物混合、一次 DIA：相对单用 trypsin，protein ID 相近，peptide +63%、非冗余氨基酸 +45%，非 tryptic 肽支持非常规 isoform。
- **Fossati A et al.** J Proteome Res 2021;20(8):4031–4040. DOI 10.1021/acs.jproteome.1c00357. 人血浆，timsTOF dia-PASEF。分开三跑再合并：14,665 peptides / 636 proteins；混合单跑：10,572 peptides / 489 proteins（肽回收率 ~73%），但特征更完整、protein CV 低 ~8 个百分点。
- **Dator RP et al.** Anal Chem 2014. DOI 10.1021/ac404020j. 先驱：trypsin/chymotrypsin/Asp-N/Glu-C 四种消化物混合、单次 LC-MSᴱ 采集、单次 library-free 搜库。注：MSᴱ 不是 windowed DIA，搜库按"trypsin 为主 + nonspecific"配置，并非明确的 union 建库——只能算先驱而非严格同类。
- 检索笔记（2026-10-12）：除上两篇外未发现其他严格符合"混合→单发→合库一次搜"的文献；18 篇引 Richards、20 篇引 Fossati 的论文中无一采用该设计。Gao et al. Anal Chem 2020 "Multi-in-One" 摘要描述了多酶 one-shot DIA 磷酸化组（~30,000 磷酸化位点），但全文付费、无法验证是分开消化后混合还是同管共消化，未计入。
