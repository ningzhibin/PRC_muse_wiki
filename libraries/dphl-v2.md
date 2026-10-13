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

---

# English

# DPHL v2 — Summary

DIA Pan-Human Library v2 — a human pan-proteome spectral library (Orbitrap data).

## Paper

Xue Z, Zhu T, Zhang F, et al. DPHL v.2: An updated and comprehensive DIA pan-human assay library for quantifying more than 14,000 proteins. *Patterns* 4(7):100792 (2023).
https://doi.org/10.1016/j.patter.2023.100792 · PMID: 37521047 · PMCID: PMC10382975

## Data sources

- 1,608 DDA files: 586 newly acquired (18 tissue types, incl. prostate cancer, liver cancer, triple-negative breast cancer, lung adenocarcinoma, esophageal cancer, GBM, healthy brain, ovarian cancer, cervical cancer, AML/T-ALL plasma, K562, etc.) + 1,022 from DPHL v1
- 24 sample types total; both FFPE and fresh-frozen samples used
- Covers >66.1% of UniProtKB/SwissProt reviewed human proteins

## Build pipeline

- MS-Fragger v3.0 search + Philosopher v3.2.9
- Two fastas: UniProt reviewed (20,350 entries) / isoform (21,997 isoforms, 42,347 entries total)
- Two digestion modes: fully specific / semi-specific (incl. N- and C-terminal semi-tryptic peptides), up to 2 missed cleavages
- Spectrum/peptide/protein three-level FDR < 0.01 (large-sample correction)
- RT alignment with CiRT (endogenous high-abundance conserved peptides via EasyPQP); v1 used synthetic iRT (SiRT)

## Four versions

2 fastas × 2 digestion modes:

| Version | Fasta | Digestion | Precursors | Peptides | Proteins |
|---|---|---|---|---|---|
| RF | reviewed | fully specific | 601,982 | 441,141 | 13,465 |
| RS | reviewed | semi-specific | 772,401 | 588,984 | 13,570 |
| IF | isoform | fully specific | 604,748 | 443,150 | 14,375 |
| IS | isoform | semi-specific | 808,672 | 624,467 | 14,555 |

- Protein counts mostly driven by the fasta; peptide counts mostly driven by digestion mode
- Brain tissue yielded the most proteins; includes 452 FDA-approved drug targets and 100 ovary-enriched proteins
- vs v1: +21.7% protein identifications, +14.2% differential proteins (RF version, colorectal cancer cohort); 1,144 proteins unique to v2

## Access (public, free)

- iProX: **IPX0005714000**
- ProteomeXchange: **PXD039313**
- All four library versions in tsv format, plus reviewed/isoform fastas and raw mzXML data

## Software compatibility

The paper claims compatibility with OpenSWATH, DIA-NN, Skyline and Spectronaut (some need format conversion). DIA-BERT loads DPHL v2 in DIA-NN 1.7.12 for library-based search. But the tsv is the output of the MS-Fragger/Philosopher/EasyPQP pipeline and is not on DIA-NN's "as is" supported list — column-name mapping via `--library-headers` may be needed. RTs in the library are on the CiRT normalized scale; DIA-NN accepts any RT scale as reference.

## v1 reference

Zhu T et al. *Genomics Proteomics Bioinformatics* 18:104–119 (2020).
https://doi.org/10.1016/j.gpb.2019.11.008
1,096 DDA files, 16 cancer sample types; 289,237 precursors / 10,943 proteins. Public and free: iProX **IPX0001400000**.
