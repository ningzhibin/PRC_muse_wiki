# 文献精读：Protein Aggregation Capture for Top-Down Proteomics

**来源**：Isabella G. Feltenstein, Bryon S. Drown（Purdue 大学化学系）。预印本 bioRxiv 2026-07-03，DOI：10.64898/2026.07.02.736076，CC-BY-NC-ND 4.0，**未经同行评审**；正式发表 *J Proteome Res* 2026，2026-10-26 在线，DOI：10.1021/acs.jproteome.6c00583——**本次精读读的是预印本 v1 全文，未核对正式版有无改动**，引用正式版时以正式版为准。数据 ProteomeXchange / MassIVE，PXD080383；分析与作图代码：https://github.com/drown-lab/pac-cleanup-tdms；补充材料含分步 protocol（S2）、裂解液制备（S3–S4）、ContamSpot 流程（S5）及全部 proteoform 命中表（.xlsx）。

**一句话**：SP3 的珠子逻辑可以延伸到完整蛋白：羟基磁珠 + 70% ACN 聚集、0.5% TFA 直接洗脱、不酶解、直接进 RPLC-MS 做 top-down。蛋白数与甲醇-氯仿-水沉淀（MCW）相当（231 vs 228），但 proteoform 数落后约两成（580 vs 735），瓶颈是珠上聚集蛋白再溶解不完全。

## 为什么做这个

- Top-down 要的是完整 proteoform（PTM 的位置、化学计量、组合），但比 bottom-up 灵敏度低，样品量常在 µg–mg 级；制备过程中的表面非特异吸附和去污剂残留是主要损失/干扰来源。
- 靶向 top-down（IP 富集后测某一蛋白家族，如 KRAS、α-synuclein、cardiac troponin、apolipoprotein、transthyretin）尤其需要这个：IP 洗脱常用 SDS（几乎什么抗原都能洗下来），但痕量 SDS 就会抑制离子化、压低低丰度 proteoform；MCW 能除 SDS，可亚 µg 量时沉淀看不见，洗涤/重悬很难做。珠子全程看得见，是这篇的卖点。
- 此前 SP3 用于 top-down 的做法：羧基珠 + 高浓度甲酸或再加一道离子交换 cleanup；或 Takemori et al. 2025 的凝胶分级 + SP3 纯化（本文 ref 5）。本文要的是一步、直接进质谱。注意：40% 甲酸的说法来自摘要转述，正文写的是羧基珠需“高浓度甲酸（如 80%）或稀 SDS 再溶解”，数字别混用。

## 方法（预印本正文 + 方法节，关键参数都以此为准）

- 羟基珠法（MagReSyn 羟基磁珠）：样品先 DTT 还原（10 mM，60 °C，30 min）→ 加羟基珠 100 µg + ACN 至 70%（v/v），1000 rpm 混匀 10 min（室温）聚集 → 磁架收珠，100% ACN 洗两次（不搅动珠-蛋白沉淀）→ 0.5% TFA 洗脱，1000 rpm 20 min，直接进样。
- 羧基珠对照（Cytiva）：50% 乙醇结合、80% 乙醇洗、0.5% TFA 洗脱（与羟基法溶剂体系不同，**不是只换珠子**，比较时注意这个混杂）。
- MCW 对照：标准流程，最终重悬于 5% ACN / 0.2% 甲酸。
- 标准品：Pierce Intact Protein Standard Mix + 四蛋白 mix（myoglobin、carbonic anhydrase、trypsinogen、ubiquitin，NRTDP standard），标准 mix 投入 4.5 µg。
- 裂解液：HeLa，50 mM HEPES pH 8.5 + 1% SDS + 蛋白酶抑制剂，探针超声，澄清后约 2 mg/mL，每条件 30 µL（约 60 µg）。
- LC-MS：Vanquish Neo + Orbitrap Exploris 480（**intact-protein mode**），MAbPac 毛细管 RP 柱（150 µm × 15 cm），1.5 µL/min，A = 0.1% FA 水，B = 0.1% FA ACN，4–64% B 约 55 min；MS1 120k，500–2000 m/z，DDA HCD top-3，MS2 60k。
- 搜库：Proteome Discoverer 3.0 SP1 + ProSight PD node 4.2，人 SwissProt（2023-06-13），high–high，precursor 2.2 Da / fragment 10 ppm，spectrum / proteoform / protein 三级 1% FDR。
- 读的版本：预印本 PDF 全文（正文 + Table 1 + 图注；补充图 S4–S8 只见正文转述，未逐图核）。

## 结果

### 1. 珠子表面化学决定成败（标准品）

- 羟基珠 + 0.5% TFA：全部标准蛋白都检出，洗脱谱图基本不变。Protein G 的 [M+23H]23+ 前体 S/N：MCW 292.91，羟基珠 212.96，羧基珠 + 0.5% TFA 只有 57.51。
- 羧基珠要 harsh 条件（文中提 80% 甲酸）才能把蛋白洗下来，但蛋白在那种条件下稳定性有限——这是换羟基珠的动机。

### 2. 稀样品回收：有改善，但不显著

- 200 ng equine myoglobin，4 ng/µL，0.5% SDS 中，MCW vs 羟基 SP3，各 6 个独立重复，ProteoQuant 定量（apex + AUC）。
- 羟基 SP3 比 MCW 高 2.4 倍（AUC）/ 3.1 倍（apex），**但不显著**（图注 p = 0.12）。结论只能写“灵敏度不吃亏 + 操作容易”，不能写“显著更灵敏”。MCW 在这个浓度下沉淀看不见，操作本身就难做。

### 3. SDS 去除：去掉了，但残留量没法定量

- ContamSpot 比色法：BSA in 50 mM HEPES + 1% SDS，清理前后对比，SDS 信号消失。
- **保留**：TFA 本身会和染料 O-toluidine 蓝强离子配对，0.5% TFA 单独对照的颜色强度与羟基 SP3 洗脱后相当，所以**残留 SDS 的精确量测不出来**，只能定性说“去除了”。引用时别写成“SDS 残留低于某值”。

### 4. HeLa 裂解液单针 top-down（Table 1，全文核心表）

| 清理方法 | 回收液 | 蛋白 | Unique proteoform | Annotated proteoform | Subsequence proteoform |
|---|---|---|---|---|---|
| MCW | 5% ACN, 0.1% FA | 228 | 735 | 480 | 429 |
| Cytiva 羧基 | 10% FA | 185 | 544 | 372 | 291 |
| Cytiva 羧基 | 20% FA | 158 | 506 | 337 | 264 |
| Cytiva 羧基 | 0.5% TFA | 204 | 560 | 380 | 311 |
| Cytiva 羧基 | 2% TFA | 105 | 431 | 276 | 262 |
| MagReSyn 羟基 | 10% FA | 206 | 572 | 405 | 306 |
| MagReSyn 羟基 | 20% FA | 186 | 470 | 325 | 259 |
| MagReSyn 羟基 | 0.5% TFA | **231** | **580** | **421** | **314** |
| MagReSyn 羟基 | 2% TFA | 173 | 486 | 321 | 298 |
| 全部条件合计去重 | | 347 | 1385 | 843 | 847 |

- 蛋白数：羟基珠 0.5% TFA 略高于 MCW（231 vs 228），但 proteoform 数落后 21%（580 vs 735）。**蛋白数相当 ≠ proteoform 水平相当**，这是全文最容易误读的点。
- 瓶颈证据：洗脱前后煮珠做 SDS-PAGE（图 S5），珠上仍有蛋白——聚集蛋白再溶解不完全，作者自己认这是限制因素。
- 酸浓度不是越高越好：2% TFA 和 20% FA 都比 0.5% TFA / 10% FA 差；高浓度有机酸还带来更多未注释 mass shift（图 S8）。
- 质量：共享 proteoform 的 P-score 分布各方法大体相似；FLASHDeconv 解卷积，MCW 和羟基珠 0.5% TFA 的高 Q-score 富集最好，5% FDR 下解卷积质量数也最多。例证：Acyl-CoA-binding protein 主 proteoform，[M+12H]12+ 的 S/N 为 MCW 41.78、MagReSyn 215.99、Cytiva 27.79——羟基珠的谱图噪声明显更低。
- 保留时间分布：proteoform 集中在梯度前半段，与疏水性/大小相关（图 S4）。

### 5. 有没有偏倚：没有可观的偏倚

- GRAVY、pI、平均质量：初看 pI 偏碱性是组蛋白 proteoform 占大头造成的；去掉组蛋白后 pI 呈典型双峰。各方法间 omnibus 效应量 η² ≤ 0.018（可忽略–小），多重检验校正后仍显著的 pairwise 差异 Cliff's δ ≤ 0.25（小）。结论：PAC 回收的 proteoform 在质量、电荷、疏水性范围上与 MCW 相当。
- Mass shift（MSTopDiff）：常见 PTM（甲基化、氧化、乙酰化、磷酸化）都见到了；三个主要方法都没有引入人工修饰；高浓度有机酸时未注释 mass shift 更多。

## 机制讨论：PAC vs HILIC，作者没把话说死

- SP3 结合机制的主流解释已从 HILIC 转向 PAC（protein aggregation capture）：聚集不依赖珠子表面类型，几乎任何珠子都行。
- 但本文显示表面化学、珠子结构、回收液都影响洗脱：Cytiva 是聚苯乙烯核 + 磁铁矿/聚合物层，MagReSyn 是磁铁矿核外 dendritic 聚合物——**表面化学的效应和核结构混在一起，没法拆开**，作者承认需要更多研究。
- 有机溶剂种类/浓度在聚集步骤中的影响也没有系统评估；binding 时加 lyotrope（促溶剂）可能改善低丰度回收，是作者列的未来方向。

## 局限（作者自述 + 阅读注记）

- 仪器端：Exploris 480 的 RPLC-MS/MS 受 ion decay、HCD 解离效率低、碎片离子谱图重叠限制；individual-ion 测量、离子淌度、其他解离方式、气相反应解卷积都可能加深 proteoform 表征——本文没做。
- 只测了两个市售珠子产品；其他市售/定制颗粒可能更好。
- 裂解液实验的重复数在正文方法节未明确写出（标准品稀释实验 n=6 写明了），引用鉴定数时注意这是单针比较的量级，别当成带误差棒的结论。
- **复现门槛**：本文用的是 Exploris 480 的 intact-protein mode，需要有该模式的仪器才能直接复现。另：本文是变性 top-down（含 SDS），与 native MS（要保住非共价复合物）是两回事，不能互相借结论。

## 与常规 SP3 的关系（我的注记）

- 同源不同终点：都是“珠 + 有机溶剂聚集、洗涤、终点处理”，但本文终点是完整蛋白酸洗脱直接进质谱，不是 on-bead 酶解；珠子（羟基 MagReSyn vs 羧基 Cytiva/AMPure）、结合溶剂（70% ACN vs 常规 95% ACN 回捕）、洗脱液（0.5% TFA）都不同，**不是把现有 bottom-up SP3 换个洗脱液就能用的**。
- 最可能用得上的场景是靶向 top-down / IP 后样品：亚 µg 投入、SDS 洗脱液、沉淀看不见时，珠子法的操作优势最大；发现型（discovery）单针 HeLa 只有 231 蛋白 / 580 proteoform 的量级，别按 bottom-up 的鉴定数预期。
- 若将来试：先以标准蛋白 mix 复现羟基珠 vs MCW 的 S/N 对比（Protein G / ACBP 例证），再动真实样品；ContamSpot 只能定性，别拿它定 SDS 残留标准。
