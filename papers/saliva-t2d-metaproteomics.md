# 文献精读：唾液蛋白质组 + 宏蛋白质组找 T2D 分子/分类学特征

**来源**：Samodova D et al., *Microbiome* 2025, 13:5（DOI 注册 2024；DOI: [10.1186/s40168-024-01997-5](https://link.springernature.com/article/10.1186/s40168-024-01997-5)；PMC: [PMC11720885](https://pmc.ncbi.nlm.nih.gov/articles/PMC11720885/)）。
作者：Diana Samodova, Evelina Stankevic, Mathilde Sonne Søndergaard, Naiyu Hu, Tarunveer S. Ahluwalia, Daniel R. Witte, Daniel Belstrøm, Anniek Frederike Lubberding, Pratik D. Jagtap, Torben Hansen*, Atul S. Deshmukh*。
单位：哥本哈根大学 Novo Nordisk Foundation Center for Basic Metabolic Research（Hansen/Deshmukh 组）+ Steno Diabetes Center Copenhagen + Aarhus 大学；Daniel Belstrøm 是口腔宏蛋白组老手（2016 PeerJ 唾液宏蛋白组 periodontitis），Pratik Jagtap 是 Galaxy-P 背景。

**一句话**：15 例新发现 T2D + 15 例匹配对照的唾液，21 分钟 diaPASEF 同时做人蛋白组和宏蛋白组；宿主免疫/脂代谢信号强，微生物层面 *Neisseria*↓、*Corynebacterium*↑，并把几条细菌肽拿去做了胰岛素分泌体外验证。

## 1. 研究设计

- **队列**：丹麦 ADDITION-PRO（2009–2011，n=2082 风险分层队列）中选 15 例 screen-detected T2D（WHO 1999 标准：空腹 + 2h OGTT）+ 15 例糖耐量正常（HC），按年龄/性别/BMI 匹配。
- **最大优点**：受试者在体检时才知道自己患糖尿病——**treatment-naive，无降糖药混杂**。这是全文方法学上最硬的一点。
- **采样**：Steno Diabetes Center Copenhagen，采集当天不刷牙，咀嚼石蜡刺激唾液分泌，立即 −80°C。**样本冻存约 11 年（2009–2011 采 → 2022 分析）**，文中称"kept un-thawed"，但 11 年冻存对唾液（蛋白酶丰富体液）蛋白完整性的影响**未被讨论**——隐藏 caveat。
- 混杂：口腔卫生、饮食、体力活动未记录；降脂/抗炎/降压药未校正。

## 2. 实验与分析方法（细节）

**样品制备**：20 µL 唾液 + 10 µL 裂解液（1% SDC / 100 mM Tris pH 8.5 / 10 mM TCEP / 40 mM CAA），99°C 煮 10 min，超声 4 min；Lys-C 1:250 1 h → Trypsin 1:100 过夜；10% TFA 1:10 终止；17,000×g 离心；StageTip C18 纯化；~200 ng 上 Evotip。**未做高丰度蛋白去除**。

**LC-MS**：Evosep One 60 SPD（21 min 梯度），8 cm PepSep 柱（150 µm ID, 1.5 µm C18），CaptiveSpray 进 timsTOF Pro 2，**DIA-PASEF**：m/z 400–1200，mobility 0.6–1.43，ramp/accumulation 100 ms，32 个 25 Da 窗口（2×16 scan 重复），cycle time ~1.8 s。

**搜库策略（值得学的一招）**：Spectronaut v17 directDIA + **16S 引导的样品特异性细菌库**：
1. 各样本 16S V4 ASV 合并去重；
2. BLASTn 比对 HOMD 16S RefSeq，保留 top 200、score > 450 的匹配；
3. 取对应 HMT-id 的全部蛋白序列 → 约 1,068,210 条细菌序列 + 人 Swiss-Prot（UP000005640，2021-06-25）。
4. 1% FDR（precursor + protein），**仅 proteotypic 肽定量**。

in silico 库：7787 proteins（2212 protein groups）/ 15,790 peptides / 24,385 precursors。

**鉴定深度**：共 4820 蛋白（1985 人 + 2835 细菌），平均每人 2231；细菌肽 7452 条；33 属、7 门。动态范围人 5 个数量级、细菌 4 个。高丰度：KRT13/KRT4/KRT6A、ANXA1、IGKC/IGHA1、细菌核糖体蛋白；低丰度：UBR4、ADPRS、NDUFB10 等。生物学重复中位 r=0.91。

**统计**：蛋白层面 log2 → 保留 ≥70% 检出 → 缺失值下移正态插补（1.8 SD downshift, width 0.3）→ t-test BH FDR<0.05；1D 富集 FDR<0.02。肽段层面用 **missForest** 插补。PCA：PC1+PC2 仅解释 29.6%，组间分离"moderate"。

**分类注释**：Unipept Desktop v2.0 LCA；47% 细菌肽能定到属，69% 到门。相对丰度 = 属内肽强度（或 16S reads）求和 / 总强度。

## 3. 主要结果

### 3.1 宿主蛋白：148 个差异蛋白（131↑ / 17↓，T2D vs HC）

上调（人）：
- **免疫/炎症**：多条免疫球蛋白可变区（IGHV3-64、IGHV1-69、IGHV3-43、IGLV3-19、IGHG3、IGKV4-1、IGLV1-51、IGLV2-8）——**约 40% 是 κ/λ 轻链**，作者自己提示这与自身免疫/肿瘤相关；补体 C3、C5、CFH、CFB；急性期蛋白 APCS、A2M、CP。
- **脂代谢**：APOE、APOB、APOA4；蛋白酶体亚基 PSMA2/3/4/6、PSMB1、PSME2。
- **质控型命中**：RBP4（胰岛素抵抗 adipokine）、MUC7 上调——与已知 T2D 生物学一致，说明流程没跑偏。

下调（人）：角蛋白 KRT4/KRT13/KRT78、角质形成细胞 sciellin（SCEL）、RPS16、细胞骨架 SH3GLB2/SPTAN1/ANXA2、酶 CILK1/PPP2R1A/NAGK。

1D 富集：免疫调控、防御反应、代谢、翻译/生物合成等。

### 3.2 细菌蛋白：差异极少（作者承认与宿主蛋白的显著变化形成对比）

- 上调：*Eubacterium* SEQF2744_00982、*Streptococcus* SEQF1066_02110（BLASTp 100% 匹配到**淀粉结合蛋白 AbpA**，参与膳食淀粉分解）、*Parvimonas* 膜脂蛋白 TmpC。
- 下调：*Neisseria* 的 Efem/EfeO 家族脂蛋白、含铜亚硝酸还原酶（BLASTp 100% 匹配 *Neisseria*）。
- **诚实但值得注意**：BLASTp 验证与 Unipept 的物种归属**不一致**（数据库差异所致），作者如实披露——说明肽→种的归属本身不稳定。

### 3.3 分类学特征（metaproteomics vs 16S）

- metaproteomics top5 属：*Prevotella、Veillonella、Actinomyces、Rothia、Fusobacterium*；16S top5：*Veillonella、Prevotella、Haemophilus、Streptococcus、Rothia*。两者 top20 相对丰度相关性 r=0.72（p<0.01）。
- 16S 差异属：*Rothia*↑（p=0.05）、*Schaalia*↑（p=0.01）、*Neisseria*↓（p=0.05）。
- metaproteomics 差异属：*Haemophilus*↑（p=0.02）、*Neisseria*↓（p=0.05）。
- **总强度 t-test：仅 *Corynebacterium* 在 metaproteomics 显著上调**，且只在 T2D 组检出（*Treponema* 只在 HC 检出）；16S 中 *Corynebacterium* 无显著差异。
- **最稳的分类学信号是 *Neisseria* 下降**（两个组学 + 蛋白层面三重一致）；*Corynebacterium* 上升只在宏蛋白组显著，16S 不支持。

### 3.4 功能：氧化应激 + 糖酵解

- 2D 富集：T2D 富集 peroxidase/AhpD-like、carboxymuconolactone decarboxylase-like——但**每个 term 只基于 2–3 条肽**（p<0.005），统计学上非常脆弱。
- *Neisseria* + *Corynebacterium* 肽聚类：*Corynebacterium* 簇富集糖酵解（GAPDH type I、NAD(P) binding）。

### 3.5 临床关联 + 体外验证（文章的"高潮"，也是最需打折的部分）

- 线性回归（校正年龄/性别/吸烟/血糖状态）：*Corynebacterium* 和 *Neisseria* 的多条肽与 **OGTT 30 min 血浆胰岛素负相关**（FDR<0.05）；*Haemophilus* 两条肽与腰围负相关、一条与体脂%负相关。glycemic status 交互模型无显著交互。
- **合成肽验证**：14 条 top 肽由 JPT 合成 → 酵母 tryptic 背景梯度掺入（1:1–1:20）→ 建 hybrid library（Spectronaut 18 Pulsar）→ 回搜原始数据：**7/14 验证成功**，并做了校准曲线定量唾液浓度。
- **INS-1 GSIS**：9 条肽各 1 µM 处理大鼠胰岛素瘤细胞；筛选阶段多条 *Neisseria* 肽"nominally"增加胰岛素分泌（多重 t-test，**未校正**）；*Neisseria* 肽 **ALLENLQEGSVIK** 在高糖（20 mM）下显著增加分泌约 25%（two-way ANOVA + Sidak）。作者自己对标：GLP-1 在 INS-1 中是 2–3 倍效应；并承认**唾液肽如何到达胰岛 β 细胞机制不明**、未测联合效应。

## 4. 批评（按重要性排序）

1. **"Precancerous state" 的解读很可能是吸烟混杂**。角蛋白下调 → 口腔鳞癌/癌前病变是全文最抓眼球的结论之一，但 Table 1 显示 T2D 组 current smokers 更多、HC 组 former smokers 更多——吸烟本身就是口腔癌前病变的最强风险因素。作者在 Discussion 里承认了这一点，但 Abstract/Conclusions 仍保留 "possible precancerous changes" 表述。没有把吸烟作为协变量校正（n=30 可能也校不动），这个结论应降级为"无法排除吸烟驱动"。
2. **合成肽验证 50% 失败率（7/14）是双刃剑**。作者包装成"targeted validation"，但另一面是：一半的候选肽在 hybrid library 回搜中没能确认——说明 directDIA 在百万级序列大库下的肽段鉴定 FDR 控制并不牢靠。宏蛋白组里"鉴定到"不等于"真的在"，这一点做方法的人要记住。
3. **微生物层面的效应量整体偏弱**。148 个差异蛋白里细菌只占 5 个；PCA 分离中等；*Corynebacterium* 的显著性只在 metaproteomics 出现、16S 不支持，且"只在 T2D 组检出"在 n=15 下很可能是检测限问题而非真实缺失。最可信的只有 *Neisseria* 下降（三重一致）。
4. **功能富集基于 2–3 条肽**。peroxidase/AhpD 富集的 term 背后各只有 2–3 条肽——这种富集结果换个数据库版本或过滤阈值就可能消失，不宜作为机制论据。
5. **肽→胰岛素分泌的因果链条缺失**。体外 1 µM 单肽、大鼠 INS-1 细胞系、筛选阶段 p 值未校正；从"唾液里有这条肽"到"它调控人体胰岛素分泌"之间隔着吸收、循环、浓度三个数量级鸿沟。Exendin（Gila monster 唾液）的类比是修辞，不是机制证据。作者承认了局限，但标题级的叙事（"microbial applications for T2D management"）跑在了证据前面。
6. **插补策略可能放大组间差异**。蛋白层面下移正态插补 + 肽段 missForest；对于系统性单侧缺失（如 *Corynebacterium* 只在 T2D 检出），插补后做 t-test 会人为制造显著性。70% presence filter 在 n=15/组下也偏宽松。
7. **11 年冻存未讨论**。2009–2011 采集、2022 分析，唾液蛋白酶丰富，降解/修饰（脱酰胺等）对定量的影响没有质控讨论。
8. BLASTp vs Unipept 物种归属不一致——肽→种水平归属不稳定，文章退回属水平是明智的；任何"种水平生物标志物"的说法都要警惕。

## 5. 亮点与可借鉴之处

- **Treatment-naive 的 screen-detected 队列**：无药物混杂，这是临床宏蛋白组里少见的干净设计。
- **16S 引导的减库策略**：用 16S ASV → BLASTn → HOMD 取蛋白，把库从"全口腔微生物"缩小到样品实际含有的分类单元，再用 directDIA。这是**临床宏蛋白组实用的建库范式**，MetaLab 这类流程可以借鉴"先 marker 基因缩库"的思想。
- **合成肽 hybrid library 回搜验证**：宏蛋白组肽段鉴定的金标准做法（JPT 合成 + 酵母背景梯度 + Pulsar 建库），虽然 50% 失败，但流程本身值得 core facility 将来做临床肽段验证时参考。
- **21 min 达到 4800+ 蛋白**：diaPASEF + Evosep 高通量路线对临床队列的示范意义；唾液无需 depletion 就能到这个深度（但角蛋白 KRT4/13/6A 高丰度污染是唾液固有问题）。
- 宿主蛋白信号（RBP4、补体、免疫球蛋白轻链、APOE/APOB/APOA4）与已知 T2D 生物学自洽，可作分析流程的阳性对照。
- 作者团队配置（口腔微生物 Belstrøm + T2D 遗传 Hansen + 蛋白质组 Deshmukh + Galaxy-P Jagtap）是"临床问题 × 组学技术"的标准组合。

## 6. 一句话评价

一篇**方法扎实、叙事偏乐观**的临床宏蛋白组 resource 型文章：宿主蛋白组部分可信度高、微生物分类学部分 *Neisseria* 下降最稳、而"细菌肽调控胰岛素分泌"和"癌前病变"两个高潮结论都存在明显混杂或证据缺口，打折阅读。最大的方法学 takeaway 是 **16S-informed 建库 + 合成肽 hybrid library 验证**这套组合拳，以及 directDIA 大库下 50% 验证失败率的警示。
