# 文献精读：Villette et al. 2025 帕金森多组学 — 宏蛋白质组（MP）层拆解

**来源**：Villette et al.（卢森堡 LCSB / Wilmes 组；Ben Kunath 为共同作者），*Microbiome* 2025, 13:200（DOI: 10.1186/s40168-025-02227-2；PMC: PMC12502400；PMID: 41053825）。2025-11-07 有 Correction（10.1186/s40168-025-02295-4），仅更新 Data Availability。

**数据**：MP 原始数据 PRIDE **PXD031457**；MG/MT 在 SRA BioProject **PRJNA782492**；整合流程 IMP 代码 gitlab.lcsb.uni.lu/IMP/imp3。

**一句话**：这篇的 MP 层是理解宏蛋白质组方法基线的经典参照——逐样本 matched 数据库思路是对的，但 Sipros + 谱图计数 + 约 3.4k 蛋白/样本的深度放在今天只是基线水平。

## 1. 研究设计

- **队列**：卢森堡 DeNoPa 队列，三组——健康对照（HC）、iRBD（前驱期）、PD（特发性帕金森病）。
- **四组学**：宏基因组（MG）、宏转录组（MT）、宏蛋白质组（MP）、宏代谢组（MM），经 IMP（Integrated Meta-omics Pipeline）整合，在 KEGG 层面连接。
- MP 执行方：UFZ 的 Jehmlich / von Bergen 团队。

## 2. MP 方法（重点）

- **样品制备**：20 µL 蛋白提取物走 **SP3**（羧基磁珠，DTT 还原 + IAA 烷基化，trypsin 1:50 过夜），**含消化后肽段回捕**——加 ACN 到 >95% 让肽重新结合磁珠，再两步洗脱：高 pH 组分（87% ACN、10 mM 甲酸铵 pH 10）+ 水相组分（2% DMSO）。**注意（2026-10-25 核查）**：Villette 是两步洗脱，但 PRIDE PXD031457 只有 128 个 RAW、文件名无 f1/f2 后缀、接近 122 个样本数，指向两步洗脱物合并后一样本一针；与 Radushev et al. 2024（Sample_f1/_f2 分开进样）不同——“两步洗脱”不等于“两针分级”。
- **采集**：**Q Exactive HF + TriVersa NanoMate（LC-chip 模式）**，trap 柱 + 25 cm PepMap 分析柱；MS1 120k、350–1550 m/z。**Methods 只写到 MS1，梯度长度与 MS/MS 参数全文缺失**——报告完整性缺口。
- **搜库**：**Sipros v1.1**（UFZ 自家工具）；数据库**逐样本自建**——同一样本 MG/MT 混合组装（IMP）预测蛋白（平均 **7.2×10⁵ 条/样本**）+ cRAP + 人 UniProt 参考蛋白组 + 反序 decoy。参数偏老派：MS1 初始容差 1 Da、后过滤 0.05 Da，MS2 0.01 Da；≥1 条 unique peptide/蛋白，动态调 peptide FDR 凑 **1% protein FDR**；定量用 **spectral count**；入组门槛 **≥2,000 蛋白/样本**。

## 3. 产出与表现——分类层拉胯、功能层还行

- **深度浅且不稳**：平均每样本 4.1×10⁴ 张谱图、鉴定 **3.4×10³ ± 1.7×10³ 个蛋白**（标准差是均值的一半）；122 个样本 MP 只保住 **104 个**（42 HC / 22 iRBD / 40 PD），其余因鉴定量低剔除。
- **taxMP 全场最弱**：组间 β 多样性不分离（PERMANOVA **p=0.35、R²=0.017**，与 taxMG、funMG 垫底），正文无 MP 层显著差异菌。
- **funMP 较好**：α 多样性 PD 显著升高；β 分离显著（**p=0.008、R²=0.041**，仅次于 funMT 的 0.043），与 funMT 是仅有的两个能区分 PD vs iRBD 的层；不受性别/年龄/便秘混杂。
- 全文头条生物学（β-glutamate 升高、鞭毛/趋化转录下调）由 MM + MT 驱动，MP 是配角。

## 4. MP 层的定位：整合层面摸到了低丰度古菌，单层统计立不住

MOFA 整合分析里疾病因子 F1 在 PD 侧富集 *M. smithii* 及“古菌蛋白与基因（基于 MP 和 MG 数据）”——原方法在整合层面摸到了 *Methanobrevibacter smithii* 的蛋白信号，但 taxMP 单层统计上立不住。这正是这类基线方法的典型画像：整合能看到信号，单层方法学还不够稳，现代 workflow 的改进空间就在这里。

**一句话**：这篇的 MP 是“2018 年代 IMP 框架下的经典做法”——逐样本 matched 数据库思路是对的，但 Sipros + 谱图计数 + 约 3.4k 蛋白/样本的深度放在今天只是基线水平。
