# 在 tip 里还是在 tip 上酶解：六种固相酶解的几何位置与代价

**整理**：2026-11-04，由当日 Evotip 结构、in-StageTip、SISPROT 讨论整理。核心问题只有一个：酶解发生时，蛋白到底在哪里——在溶液里、在固相表面、在固相内部，还是在固相孔隙里？位置决定了蛋白酶能不能碰到蛋白、要多长时间、以及肽段最终落在何处。

## 六种位置对照

- **One-Tip / Evotip：液相，盘上方**——消化液坐在 C18 盘的上表面，不渗入填料床；C18 只在最后一步起捕获作用。
- **in-StageTip / C18：盘内截留**——蛋白被截留在 C18 盘内部、广泛分布；酶解在盘里进行，需要过夜。
- **AnExSP / AX：盘表面**——蛋白分层定位在 AX 盘表面、不渗入盘内；表面富集便于蛋白酶接近，1–4 小时可完成消化。
- **SISPROT / SCX：三段接力，pH 开关**——低 pH 结合捕获，高 pH 释放后在 tip 内液相酶解，pH 10 再转移到 C18。
- **SP3 / PAC：珠上结合态酶解**——蛋白结合在磁珠上，酶解时仍结合在珠子上。
- **S-Trap：基质内酶解**——蛋白截留在纤维/膜基质孔隙中，酶解在基质内部进行。

## One-Tip：消化液坐在盘的上表面

Evotip 底部是一小段 C18 复合盘：树脂颗粒包埋固定在多孔基质里，功能上类似 Empore 盘；Evosep 专利（US12298284B2）写的是树脂“embedded and fixated therein”，全文没有出现 "frit" 一词——即没有上下筛板夹填料的结构，盘本身靠 tip 的锥度卡住。

2024 细胞版（Nat Commun s41467-024-46777-9）的操作把“盘上方液相酶解”体现得最清楚：

- 加 5 µL 裂解/消化液 + 5 µL 细胞悬液后，只用 **50 g 短暂离心**，目的是让液体贴合 C18 表面、赶气泡，不压穿；
- **800 g × 60 s 才是压穿档位**，用于预处理（20 µL Solvent B 润洗、20 µL Solvent A 平衡）与消化后上样（50 µL Solvent A）；
- 孵育时向 Evotip 盒内加水至 C18 高度，出口浸没、无压差、不蒸发，液体就稳定地坐在盘上；
- 800 g 在加消化液后立即离心，对 One-Tip 是操作错误：反应体积会被提前压穿、结合不均匀。

血浆版（Logerot et al., Anal Chem 2026）的酶解也在盘上方溶液里做：10 µL 0.2% DDM / 100 mM TEAB / 10 mM TCEP、37 °C 30 min 变性，2 µL IAA（终浓度 44 mM）室温暗处 30 min 烷基化，3 µL 胰蛋白酶、酶底比 1:14、37 °C 4 h 酶解；之后加 50 µL 0.1% FA、800 g 60 s 上样到 C18。注意 Evotip 不耐受 37 °C 以上，这是血浆版不用更高温度的原因之一。

## in-StageTip：蛋白在 C18 盘内部被切

Kulak et al.（Nat Methods 2014，Mann 组）的 in-StageTip 把从细胞裂解到纯化肽段洗脱的全部步骤放在一根装有 C18 Empore 盘的 tip 内。蛋白在盘里的位置，是 Takemori 等 2022 年用荧光蛋白 b-phycoerythrin 直接验证的：**在 C18 RP StageTip 里，蛋白被截留在盘内部、呈广泛分布**。

后果是时间：所有沿用该方法的 protocol 酶解都是一夜——Wageningen 改良版是胰酶溶液室温振荡过夜，另一篇应用文献是 LysC + 胰酶 37 °C 过夜。终点深度不差（Kulak：人细胞系四次重复，9,667 个蛋白拷贝数估计，重复间 R² = 0.97），但瓶颈是蛋白酶要扩散进盘内才能碰到蛋白。Kulak 原文没有发表专门的酶解动力学指标（底物消失时间曲线、漏切率），这一点不要反推。

## AnExSP：蛋白在 AX 盘表面被切

Takemori et al.（Chem Commun 2022）改用单张 AX（阴离子交换）盘，动机是 C18 体系不耐 SDS 和考马斯亮蓝（CBB）。定位实验的另一半结果是：**在 AX 盘上，蛋白以分层方式定位在盘表面、没有渗入盘内**，作者的结论是表面富集“便于蛋白酶接近”。

酶解动力学只在 AX 盘上测过，数字很干净：10 µg 人细胞蛋白提取物，Trypsin/Lys-C，底物∶酶 20∶1 时 30 分钟内大部分底物消失、1–4 小时完全消化（25 °C）；100∶1 时 4 小时消化不完全；DIA 结果是 4 小时鉴定约 7,000 个蛋白，与过夜 18 小时没有差别。SDS 处理上，SDS 与蛋白一起截留在盘上，肽段用 0.5% 甲酸 / 30% 乙腈洗脱时 SDS 不跟下来；但 SDS 浓度过高时蛋白会渗入盘内，作者明确写这会“使盘上的消化变得困难”——这是“渗入 = 难消化”的直接证据，也反衬了 C18 盘内分布的劣势。

注意命名：这篇论文通篇用 AX（SI 明确是 3M Empore AX SPE disc），没有出现 SAX 一词；它也没有做 AX 与 C18 的消化效率正面比较，不要引用成“SAX 比 RP 消化得好”。

## SISPROT：SCX 捕获、液相酶解、C18 捕获肽段

SISPROT（Chen et al., Anal Chem 2016，Tian 组）的 tip 是上层 SCX 珠子 + 下层 C18 盘。Lin et al. 2018 发表的流程把 pH 线写得非常清楚：

- **pH 2–3 上样**：样品用 0.1% 甲酸酸化到 pH 2–3 再上 tip，蛋白带正电，结合在 SCX 珠子上；
- **pH 3 洗涤、pH 3 还原**：20% 乙腈 / 8 mM 柠檬酸钾（pH 3）洗，10 mM TCEP / 9 mM 柠檬酸钾（pH 3）还原 15 分钟——还原特意放在 pH 3 做，就是为了不让蛋白提前下来；
- **pH 8 酶解**：胰酶溶在 100 mM Tris-HCl（pH 8，含 10 mM IAA，烷基化和酶解同时做）里打进 tip，室温 1 小时；
- **pH 10 转移**：200 mM 甲酸铵（pH 10）把肽段从 SCX 转到下层 C18 盘上，再洗脱。

关键纠正：**SISPROT 的酶解不是结合态表面酶解**。pH 8 时蛋白从 SCX 上释放下来，酶解在 tip 内的液相里进行；其前身 proteomic reactor 文献明说 pH 8 时“蛋白从 SCX 珠子上释放下来、被胰酶消化”。释放不等于丢失：液体没有流出 tip，酶解完再用 pH 10 把肽段主动转到 C18 上兜底。autoSISPROT 论文（Wu et al., Chem Sci 2024）那句 “digestion on the beads” 是简化表述，不能按字面理解。细节上，pH 8 时高 pI 蛋白仍带正电、释放得慢，是“边切边释放”，但终点都是 C18 捕获。

SISPROT 家族还有两个变体：原始版用 SCX 珠子；3D-SISPROT（Chen et al., J Chromatogr A 2017）用 SAX 膜 + C18 膜做盘内消化与三维分级；autoSISPROT 用 SCX/SAX 混合珠子。

## SP3 / PAC 与 S-Trap：另外两种结合态/基质内酶解

- **SP3**：蛋白结合在羧基磁珠上，酶解时仍结合在珠子上；消化后 PRC 现行做法是 95% ACN 肽段回捕（见本节另一篇 [SP3 肽段回捕](sample-prep/sp3-peptide-cleanup.md)）。BAMM 多组学版改用未修饰硅珠（Cytiva SeraSil-Mag 700），酶解用 Rapid Trypsin、60 °C 40 分钟、全程 1000 rpm 振荡——选未修饰珠的原因是羧基珠会捕获极性代谢物，不是蛋白结合更好。
- **S-Trap**：蛋白截留在石英纤维/膜基质孔隙中，酶解在基质内部进行；One-Tip 血浆版论文里 S-Trap 对照在 37 °C 过夜做，就是因为 Evotip 不耐受 Protifi 标准的 47 °C / 1 h 条件。

## 经验规律：问“酶解在哪里”比问“酶解在什么上”更准

- 看到方法写 “on-bead / on-disc digestion”，先问两个问题：酶解时蛋白还结合在固相上吗？酶解液的 pH 相对结合 pH 变了没有？SISPROT 的 pH 8 释放就是反例：字面上是“珠上酶解”，实际上是液相酶解。
- 可及性决定时间（整理者的判断，非文献原话）：液相（One-Tip，4 h）与盘表面（AX，1–4 h）最快；珠上结合态（SP3）居中；盘内截留（C18 in-StageTip）与基质内（S-Trap）最依赖扩散或长时间。选方法时，时间预算和“是否接受过夜”往往比绝对鉴定数更先决定取舍。
- 固相的角色要分开看：同一根 tip 里，固相可以在不同步骤扮演不同角色——SISPROT 里 SCX 管捕获、C18 管肽段脱盐；One-Tip 里 C18 在酶解时只是容器底，酶解后才变捕获柱。把“反应器”和“捕获柱”混为一谈，是这类方法最常见的误读。

## 来源

- Kulak NA, Pichler G, Paron I, Nagaraj N, Mann M. Minimal, encapsulated proteomic-sample processing applied to copy-number estimation in eukaryotic cells. *Nat Methods* 2014;11:319–324. DOI 10.1038/nmeth.2834.
- Takemori A, Kawashima Y, Takemori N. Bottom-up/cross-linking mass spectrometry via simplified sample processing on anion-exchange solid-phase extraction spin column. *Chem Commun* 2022;58:775–778. DOI 10.1039/d1cc05529a.
- Chen W, Wang S, Adhikari S, Deng Z, Wang L, Chen L, Ke M, Yang P, Tian R. Simple and integrated spintip-based technology applied for deep proteome profiling. *Anal Chem* 2016;88:4864–4871. DOI 10.1021/acs.analchem.6b00631.
- Lin L et al. Fast quantitative urinary proteomic profiling workflow for biomarker discovery in kidney cancer. *Clin Proteomics* 2018;15:42（SISPROT 流程细节来源）.
- Chen W et al. 3D-SISPROT. *J Chromatogr A* 2017;1498:207–214. DOI 10.1016/j.chroma.2017.01.033.
- Wu Q et al. High-throughput drug target discovery using a fully automated proteomics sample preparation platform (autoSISPROT). *Chem Sci* 2024;15:2833–2847. DOI 10.1039/D3SC05937E.
- Logerot E et al. One-Tip plasma proteomics. *Anal Chem* 2026. DOI 10.1021/acs.analchem.6c04223（血浆版）；2024 细胞版 *Nat Commun* s41467-024-46777-9.
- Evosep ApS 专利 US12298284B2（Evotip 树脂包埋固定、无筛板措辞来源）；Kverneland et al. *Mol Cell Proteomics* 2024;23:100790（EvoTip 为 specialized StageTip 的表述来源）.

## 仍未核实

- Kulak 原文酶解细节：全文付费墙，只读到摘要与参考文献；本文酶解条件引自 Wageningen 改良版 protocol 与一篇应用文献的补充材料，不是 Kulak 原文方法。
- Evotip 复合盘里包埋树脂的基质材料：Evosep 未公开。
- SISPROT 高 pI 蛋白“边切边释放”的渐进程度：未量化，本文只写机制方向。
