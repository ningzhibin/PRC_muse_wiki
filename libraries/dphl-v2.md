# DPHL v2

DIA Pan-Human Library v2 —— 人 pan 蛋白谱图库。

## 文献

Xue Z, Zhu T, Zhang F, et al. DPHL v.2: An updated and comprehensive DIA pan-human assay library for quantifying more than 14,000 proteins. *Patterns* 4(7):100792 (2023).
https://doi.org/10.1016/j.patter.2023.100792 · PMID: 37521047 · PMCID: PMC10382975

## 规模

- 1,608 个 DDA 文件，24 种样本类型（含多种癌症）
- 四个版本：RF / RS / IF / IS（是否含蛋白 isoform × 是否含半胰酶切肽）
- RF 版：601,982 precursors / 441,141 peptides / 13,465 proteins
- IF 版：604,748 precursors / 14,375 proteins（含 isoform）
- 含 452 个 FDA 批准药物靶点

## 获取（公开免费）

- iProX: **IPX0005714000**
- ProteomeXchange: **PXD039313**
- 四个版本谱图库为 tsv 格式，另有 fasta 和原始数据

## 在 DIA-NN 中使用

可用。DIA-BERT 原文即用 DIA-NN 1.7.12 加载 DPHL v2 做 library-based search。但 tsv 是 MS-Fragger/Philosopher/EasyPQP 流程产出，不在 DIA-NN "as is" 支持名单里，可能需要 `--library-headers` 映射列名。库内 RT 为 CiRT 归一化尺度，DIA-NN 接受任意 RT 尺度做参考。

## v1

Zhu T et al. *Genomics Proteomics Bioinformatics* 18:104–119 (2020).
https://doi.org/10.1016/j.gpb.2019.11.008
1,096 个 DDA 文件，16 种癌症样本；289,237 precursors / 10,943 proteins。公开免费：iProX **IPX0001400000**。
