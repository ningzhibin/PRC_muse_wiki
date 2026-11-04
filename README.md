# PRC Muse Wiki

蛋白质组学知识库。每个知识点是一个独立的 Markdown 文件。

- **Obsidian**：把这个文件夹直接打开为 vault 即可使用
- **在线浏览**：通过 GitHub Pages（Docsify 渲染，无需构建）

## 知识点

### 文献精读

- [唾液蛋白质组 + 宏蛋白质组找 T2D 分子/分类学特征](papers/saliva-t2d-metaproteomics.md)
- [MassNet：十亿级 AI-ready 质谱语料库](papers/massnet-billion-scale-corpus.md)
- [组蛋白丙酰化优化 + DIA 窗口方案比较（N34 的出处）](papers/histone-propionylation-dia-n34.md)
- [Villette et al. 2025 帕金森多组学 — 宏蛋白质组（MP）层拆解](papers/villette-2025-parkinson-multiomics.md)
- [DDA / DIA de novo 测序现状 — Casanovo 系与 Cascadia](papers/denovo-dda-dia-casanovo-cascadia.md)
- [Suresh & Zhang 2025 — 9 种血浆 EV 分离方法全面比较](papers/suresh-zhang-2025-ev-isolation-comparison.md)
- [Protein Aggregation Capture for Top-Down Proteomics](papers/feltenstein-drown-2026-pac-topdown.md)
- [Rapid multi-omics sample preparation for mass spectrometry（BAMM）](papers/muehlbauer-2023-bamm-multiomics.md)

### DIA

- [打分模型的训练策略：全局预训练 vs 逐文件学习](dia/scoring-pretrain-vs-perrun.md)
- [DIA-BERT 笔记](dia/diabert-notes.md)
- [FDR 控制的层次与语境](dia/fdr-levels-and-contexts.md)
- [多酶混合单发 DIA：一次混合消化物采集 + 联合库一次搜库](dia/pooled-multi-enzyme-dia.md)
- [ZT Scan DIA 的 1342 Hz 是怎么算出来的：扫描速率的口径问题](dia/zt-scan-dia-scan-rate.md)

### 谱图库

- [DPHL v2](libraries/dphl-v2.md)

### 深度学习

- [Gradient 到底是什么：导数、偏导数与代码里的 grad](deep-learning/gradient-grad.md)

### 肽聚糖

- [PGLang 速查：给做蛋白质组学的人](peptidoglycan/pglang-cheatsheet.md)

### 翻译后修饰

- [S-酰化富集（Acyl-RAC）不需要特殊的搜库参数](ptm/s-acylation-acylrac-search-params.md)

### 色谱

- [超快低流速 LC-MS：柱后死体积是隐形杀手](lc/post-column-dead-volume.md)

### 细胞外囊泡

- [ExoCarta、Vesiclepedia vs MISEV2023：两个数据库，一个指南](ev/exocarta-vs-misev2023.md)

### 样品制备

- [在 tip 里还是在 tip 上酶解：六种固相酶解的几何位置与代价](sample-prep/in-tip-digestion-geometries.md)
- [SP3 肽段回捕：消化后 95% ACN 这一步要不要留](sample-prep/sp3-peptide-cleanup.md)
