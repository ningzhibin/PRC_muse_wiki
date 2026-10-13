# DPHL v2 数据库总结

DIA Pan-Human Library v2 —— 人 pan 蛋白谱图库（Orbitrap 数据）。

## 文献

Xue Z, Zhu T, Zhang F, et al. DPHL v.2: An updated and comprehensive DIA pan-human assay library for quantifying more than 14,000 proteins. *Patterns* 4(7):100792 (2023).
https://doi.org/10.1016/j.patter.2023.100792 · PMID: 37521047 · PMCID: PMC10382975

## 数据来源

- 1,608 个 DDA 文件：586 个新采（18 种组织类型，含前列腺癌、肝癌、三阴性乳腺癌、肺腺癌、食管癌、GBM、健康脑、卵巢癌、宫颈癌、AML/T-ALL 血浆、K562 等）+ 1,022 个来自 DPHL v1
- 共 24 种样本类型；FFPE 和新鲜冰冻样本都用了
- 覆盖 UniProtKB/SwissProt 已审核人蛋白的 66.1% 以上

## 构建流程

- MS-Fragger v3.0 搜库 + Philosopher v3.2.9
- 两个 fasta：UniProt reviewed（20,350 条）/ isoform（含 21,997 条 isoform，共 42,347 条）
- 两种酶切模式：全特异性 / 半特异性（含 N 端和 C 端半胰酶切肽），最多 2 个漏切
- 谱图/肽段/蛋白三层 FDR < 0.01（大样本量校正）
- RT 对齐用 CiRT（EasyPQP 内源高丰度保守肽），v1 用的是合成 iRT（SiRT）

## 四个版本

2 种 fasta × 2 种酶切模式：

| 版本 | fasta | 酶切 | precursors | peptides | proteins |
|---|---|---|---|---|---|
| RF | reviewed | 全特异 | 601,982 | 441,141 | 13,465 |
| RS | reviewed | 半特异 | 772,401 | 588,984 | 13,570 |
| IF | isoform | 全特异 | 604,748 | 443,150 | 14,375 |
| IS | isoform | 半特异 | 808,672 | 624,467 | 14,555 |

- 蛋白数主要受 fasta 影响，肽段数主要受酶切模式影响
- 脑组织检出蛋白最多；含 452 个 FDA 批准药物靶点、100 个卵巢富集蛋白
- 与 v1 相比：鉴定蛋白数 +21.7%，差异蛋白 +14.2%（RF 版，结直肠癌队列）；1,144 个蛋白为 v2 独有

## 获取（公开免费）

- iProX: **IPX0005714000**
- ProteomeXchange: **PXD039313**
- 四个版本谱图库为 tsv 格式，另附 reviewed/isoform 两种 fasta 和原始 mzXML 数据

## 软件兼容性

原文称兼容 OpenSWATH、DIA-NN、Skyline、Spectronaut（部分需格式转换）。DIA-BERT 即用 DIA-NN 1.7.12 加载 DPHL v2 做 library-based search。但 tsv 是 MS-Fragger/Philosopher/EasyPQP 流程产出，不在 DIA-NN "as is" 支持名单里，可能需要 `--library-headers` 映射列名。库内 RT 为 CiRT 归一化尺度，DIA-NN 接受任意 RT 尺度做参考。

## v1 对照

Zhu T et al. *Genomics Proteomics Bioinformatics* 18:104–119 (2020).
https://doi.org/10.1016/j.gpb.2019.11.008
1,096 个 DDA 文件，16 种癌症样本；289,237 precursors / 10,943 proteins。公开免费：iProX **IPX0001400000**。
