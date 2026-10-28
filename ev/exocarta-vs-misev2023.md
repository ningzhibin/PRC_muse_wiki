# ExoCarta、Vesiclepedia vs MISEV2023：两个数据库，一个指南

三者常被并列提起，但**不是同一类东西**。ExoCarta 和 Vesiclepedia 都是数据汇编，回答"以前在 EV 里报导过什么分子"；MISEV2023 是共识指南，回答"一项 EV 研究应该怎么做、怎么报导"。MISEV2023 全文（已核查，2026-10-28）**一次也没有提到 ExoCarta 或 Vesiclepedia**；它建议用 UniProt 的亚细胞定位/特征信息来归类候选 marker 蛋白，而不是用这两个数据库。

| | ExoCarta | Vesiclepedia | MISEV2023 |
|---|---|---|---|
| 是什么 | 人工整理的 cargo 数据库（蛋白、RNA、脂质） | 人工整理 + 社区注释的 cargo 数据库（蛋白、RNA、脂质、代谢物），附实验质量信息 | ISEV 的立场文件/指南：最低信息要求 + 建议 |
| 范围 | 外泌体 / sEV | **所有 EV 类别**（外泌体、ectosome/微囊泡、凋亡小体，及更新的 EP 类别） | 所有 EV/EP 研究 |
| 回答的问题 | 某分子以前是否在 sEV 里报导过？出现频率如何？ | 某分子在各类 EV 里报导过吗？来源研究的分离/表征方法与报告透明度如何？ | 命名、取样、分离、表征、功能实验应如何做和报导？ |
| 含不含数据 | 含，按研究/分子/物种/样品类型可查 | 含，可按样品来源、分离方法、EV 亚型、物种过滤 | 不含任何 cargo 数据 |
| 版本 | 最后发表的更新是 2016 年（见下，时效存疑） | 最后发表的更新是 2019 年（见下，时效存疑） | 2024 年发表；前身 MISEV2014、MISEV2018 |
| 引用 | Keerthikumar et al., *J Mol Biol* 2016;428:688–692 | Pathan et al., *Nucleic Acids Res* 2019;47:D516–D519 | Welsh et al., *J Extracell Vesicles* 2024;13:e12404 |

## ExoCarta

**历史。**2009 年 Mathivanan & Simpson 在 *Proteomics* 首次提出；2012 年 *Nucleic Acids Res* 数据库专刊（ExoCarta 2012）；2016 年 *J Mol Biol* 更新版（Keerthikumar et al.）——这是能找到的最后一次发表的更新。维护方为 La Trobe 大学 Mathivanan 实验室。

**范围。**首页现称"小细胞外囊泡（sEV）蛋白、RNA 和脂质数据库"，sEV 包括内吞起源的 30–150 nm 外泌体和从质膜脱落的小外泌体（small ectosomes）。注意名称与 MISEV2023 命名建议之间的张力：库名用 "exosome"，但按 MISEV2023，除非能证明内体起源，否则应称 EV/sEV；库里的大多数条目实际来自 broad 的 EV 群体，而非严格证明起源的外泌体。

**内容（2016 年快照，不是现况）。**2016 年论文的数字：**41,860 个蛋白、>7,540 个 RNA、1,116 个脂质分子，来自 >286 项研究**，条目按当时 ISEV 最低实验要求注释。同一篇论文称近 25% 的人类蛋白已知经由外泌体分泌。可查询/浏览的维度：基因符号、蛋白名、miRNA ID、物种、内容类型、样品材料；附蛋白–蛋白互作网络和通路；可按"被多少项研究报导"下载最常鉴定的蛋白，导入 FunRich 做富集/网络分析。

**时效警告（2026-10-28 核查）。**ExoCarta 首页不显示任何统计数字、版本号或更新日期；统计页面 `ExoCarta_Statistics.html` 在真实浏览器中返回 404/资源不存在。没有找到更新的发表版本。因此 **41,860 等数字只能作为 2016 年快照引用，使用时必须到网站上现查，不得当作当前收录量**。数据库条目大多是定性"出现过"的报导，方法学异质性大：**出现在 ExoCarta 里 ≠ marker，≠ EV 特异，更 ≠ 富集**。

**Top 100 / Top 1000 是什么。**按"被多少项独立研究报导"排序的频率榜，不是丰度榜，也不是富集榜。用法示例（Wu et al. 2025，MagNet 血浆 sEV）：Top 1000 中检出 729 个，其中 76% 相对未分级血浆富集；Top 100 全部检出，87% 富集。正确用法是把它当频率参照，再用自己的富集/耗竭数据（相对起始材料）和 MISEV 五类框架去判断。

**与 Vesiclepedia 的关系。**见下节：同一课题组（La Trobe，Mathivanan 实验室）维护的姊妹库，Vesiclepedia 范围更广。EVpedia（Kim et al.）是另一个独立资源，不要与 Vesiclepedia 混淆。

## Vesiclepedia

**历史。**Kalra et al., *PLoS Biol* 2012;10:e1001450 创刊，定位就是"社区持续注释"——针对当时 >50% 的数据库不定期更新、>20% 链接失效的问题，发起社区注释项目防止数据库衰变；2019 年 *Nucleic Acids Res* 更新版（Pathan et al.）——这是能找到的最后一次发表的更新。维护方与 ExoCarta 相同（La Trobe 大学 Mathivanan 实验室，另有 Ghent 大学 Hendrix 实验室参与 2019 版）。

**范围：比 ExoCarta 广。**2019 版论文原话：相比以外泌体为主的 ExoCarta，Vesiclepedia 收录**所有类别 EV**中鉴定的蛋白、RNA、脂质和代谢物——外泌体、ectosome/脱落微囊泡、凋亡小体；原核与真核来源都包括；已发表与未发表研究都收（未发表数据可设私有直至发表）。2026-10-28 核查的官网首页措辞已跟上 MISEV2023 时代：亚型列表还包括 migrasome、exopher、大 oncosome，并把 exomere/supermere 列为 extracellular particle（EP）。

**内容（2019 年快照，不是现况）——注意"条目"与" unique 分子"的区别。**2019 年论文 Table 1：**1,254 项 EV 研究；蛋白条目 349,988 条，但 unique 蛋白 32,267 个；RNA 条目 38,146 条（mRNA 27,646 条/17,005 unique；miRNA 10,520 条/2,431 unique）；脂质/代谢物条目 639 条；41 个物种、172 种样品来源**。其中 648 项研究为人工注释，其余为社区贡献。所谓"条目"是"某研究中报导一次"计一次，**349,988 绝不能拿来与 ExoCarta 的 41,860 个蛋白直接比大小**。按分离方法统计：超速离心占 58%（计入联合方法则微分离心 >73%），商业试剂盒/柱子约 7%。

**Vesiclepedia 独有的两样东西，ExoCarta 没有。**

- **EV-METRIC / EV-TRACK 链接**：每项研究的实验页面附 EV-METRIC 评分——按该研究报导的分离与表征参数衡量"报告透明度"，并链接到 EV-TRACK 网站说明评分依据。这是判断一条历史 cargo 记录可信度的现成工具：一条蛋白记录来自 EV-METRIC 高的研究，还是只报了"超速离心 + Western"的研究，含金量不同。
- **FunRich 插件 + 过滤下载**：整个数据库可经 FunRich 插件自动更新、做 Venn 图或富集分析；下载时可按样品来源（如癌症/干细胞）、分离方法（如密度梯度离心）、EV 亚型（如 ectosome）、物种过滤。对 PRC 来说，这意味着可以只拿"血浆 + 亲和/SEC 分离"的研究做参照，而不是拿所有 EV 研究混在一起比。

**Top 100 警告——作者自己写的。**2019 版论文" pitfalls"一节原意：Vesiclepedia 的 Top 100 蛋白"仅仅意味着这些蛋白在 EV 研究中最常被报导，**不能因此称之为 EV marker 或富集蛋白**；其中一些是真正的 EV 富集蛋白，另一些可能是实验假象，在 EV 中丰度很低。"同一警告同样适用于 ExoCarta 的 Top 100/1000。作者还强调：数据库质量取决于原始研究，EV 命名缺乏全球共识、实验室间 protocol 差异大，跨研究比较并不简单。

**时效警告（2026-10-28 核查）。**Vesiclepedia 官网（microvesicles.org）可以正常打开，首页措辞是最新的，但首页同样不显示任何统计数字、版本号或更新日期；没有找到更新的发表版本。因此 **2019 年数字只能作为快照引用，使用时须到网站现查**。与 ExoCarta 相比，Vesiclepedia 至少网站本身是活的（ExoCarta 统计页已 404），但"活着"不等于"数据最新"。

## ExoCarta vs Vesiclepedia：选哪个？

|  | ExoCarta | Vesiclepedia |
|---|---|---|
| 查什么 | sEV/外泌体 cargo，频率参照（Top 100/1000 最常用） | 各类 EV cargo，可按亚型/方法/物种过滤的参照 |
| 代谢物 | 不收 | 收（与脂质合计 639 条目，2019 年） |
| 实验质量信息 | 条目按 MISEV2014 时代最低要求注释 | 分离/表征方法 + EV-METRIC 评分 + EV-TRACK 链接 |
| 互作/通路 | 蛋白–蛋白互作网络和通路 | FunRich 富集/网络（经插件） |
| 最新发表更新 | 2016 年 | 2019 年 |
| 网站状态（2026-10-28） | 首页可打开，统计页 404 | 首页可打开，无统计数字 |

实际使用中两者不是二选一：先用 Vesiclepedia 按自己的样品类型/分离方法过滤出可比的研究集合，再用 ExoCarta Top 100/1000 做 sEV 频率对照；两个库的"常被报导"名单高度重叠时，结论才稳。两个库都不提供定量富集信息，富集判断永远来自自己的对照实验。

## MISEV2023

**出处与规模。**Welsh et al., *J Extracell Vesicles* 2024;13(2):e12404，PMID 38326288，PMCID PMC10850029；勘误 2024;13(5):e12451。三年制定：2020 年预调查、五人组织委员会、2022 年调查 >1,000 人回应、最终 **1,051 位共同作者、53 个国家**。分 9 个部分：命名；非囊泡细胞外颗粒（NVEP，新）；采集/前处理（含细菌、体液、组织，新）；分离与浓缩；表征；技术特异性报导（新）；释放与摄取（新）；功能研究；体内研究（新）。

**命名（§2，对蛋白质组学报导影响最大）。**

- EV 定义：细胞释放、由脂质双层界定、不能自我复制（无功能细胞核）的颗粒。
- 推荐总称：EV；上位总称 extracellular particle（EP，含 EV + NVEP）；NVEP = 无脂质双层的多分子组装体（脂蛋白、RNP、病毒、exomere/supermere 等常与 EV 共分离）。
- "exosome"（内体起源）、"ectosome"/"microvesicle"（质膜起源）是**生物发生术语**，除非能证明亚细胞起源，否则不鼓励使用；大多数既有文献实际研究的是 broad EV 群体。
- 操作性术语（small EV 通常 <200 nm、large EV >200 nm）可用，但须谨慎并明确定义，因为分离方法得到的群体尺寸分布常重叠。**sEV ≠ exosome**：小 EV 群体同时包含小 ectosome 和外泌体。

**表征（§5）。**定量 + 定性结合：报告完整粒径分布而非仅均值/众数/中位数；总蛋白定量（比色/荧光/胶染色/吸光度）**会因共分离蛋白而高估 EV 量**，不建议用作 EV 浓度的替代指标，蛋白/颗粒比须同时报告两个原始量；形态学以电镜/冷冻电镜/AFM 为佳（干燥制样可产生假的杯状形态）。

**蛋白组成的五类框架（§5.7，Table 3）。**MISEV2023 明确表示：由于 EV 异质性，**不能推荐任何 EV 亚型的特异性分子 marker**。报导蛋白含量主张时用以下五类框架，理想情况是展示 marker 相对未分级起始材料的富集或耗竭：

| 类别 | 含义 | 例子 |
|---|---|---|
| 1. 跨膜/GPI 锚定蛋白（质膜和/或内体） | EV 存在的证据 | 1a 多次跨膜：四跨膜蛋白 CD9、CD63、CD81、CD82；1b 单次跨膜：MHC I/II、整合素、LAMP1/2、TFRC；1c GPI/脂锚定：GPC1、CD73（NT5E）、CD59 |
| 2. 胞质蛋白 | EV 存在的证据 | 2a 结合脂/膜蛋白：ESCRT（TSG101、CHMP）、ALIX（PDCD6IP）、VPS4、FLOT1/2、syntenin（SDCBP）；2b 广泛掺入：HSPA8、HSP90AB1、ACT、TUB、GAPDH（注意：也大量存在于 NVEP） |
| 3. 非 EV 共分离结构的主要成分 | 纯度对照 | 3a 脂蛋白（载脂蛋白）；3b 蛋白/蛋白-核酸聚集体：免疫球蛋白、UMOD、albumin、YWHA、AGO；3c exomere/supermere 富集成分：HSP90AA/B、TGFBI、LDHA/B |
| 4. 其他细胞内区室的蛋白 | 亚型/病理/新方法时参考，可选 | 组蛋白、LMNA/C（核）；VDAC、CYCS、TOMM20（线粒体）；CANX、HSP90B1、HSPA5、GOLGA2（内质网/高尔基）；MAP1LC3A、ACTN1/4（自噬体/骨架） |
| 5. 与 EV 一起回收的分泌蛋白 | corona 或功能成分 | 载脂蛋白/补体/纤维蛋白原；细胞因子/生长因子（TGFB、IFNG、VEGFA）；FN1、胶原、LGALS3BP、AHSG |

规则：**至少分析第 1、2、3 类各一个蛋白**；第 4 类可选；第 5 类蛋白可能在释放后结合到 EV 表面（corona）。表中只给少量示例，其他蛋白按 UniProt"亚细胞定位"和"特征"（跨膜结构域、拓扑）归类。

**质谱蛋白质组学（§6.3）。**非靶向（ToF/Orbitrap，发现式，全面反映样品组成含污染蛋白，适合 biomarker 发现）与靶向（QQQ，针对预设阈值的有/无判断和绝对定量，SIL 肽段标准品 + 匹配基质校准）各有适用场景。报导要求：遵循 MIAPE；逐步报告样品制备；报告软件及版本、鉴定/定量的过滤标准与置信水平、定量方法；**归一化依据**（总蛋白、起始材料体积或颗粒数）必须说明；数据与元数据上传至公共库。靶向分析另要求：阳性/阴性对照、每肽至少 1 个定量 + 1–2 个定性离子对、匹配基质中定义线性/LOD/LOQ、报告靶肽序列与选择策略。

**MISEV 不是什么。**ISEV 官网说明：它不是进入该领域的不合理门槛，不是压制创新，也不是只适用于哺乳动物/临床研究。它规定的是**最低报导要求**，不指定某一种分离方法为标准。

## 三者如何配合（PRC 工作流视角）

1. **用 MISEV2023 设计和报导**：命名用 EV/sEV + 操作性定义；分离方法、起始材料量、前处理、表征方法（粒径分布 + 形态 + 五类蛋白）全部记录；质谱部分按 §6.3 + MIAPE 报导并 deposition 数据。
2. **用 Vesiclepedia 找可比的历史研究**：按样品来源、分离方法、EV 亚型、物种过滤，重点看与自己制备方式相近的研究，并用 EV-METRIC 判断那些研究的报告透明度；不要拿全库频率当自己的预期值。
3. **用 ExoCarta 做 sEV 频率参照**：把鉴定列表与 ExoCarta Top 100/1000 对照，看"常被报导的 EV 相关蛋白"检出比例；但结论必须来自**相对起始材料的富集/耗竭**，不是检出数量本身。
4. **血浆 sEV 的判断标准**：第 1/2 类 marker 富集 + 第 3 类（ALB、APOB 等）耗竭才是富集证据；总鉴定数单独不能说明富集成功。两个实例：Wu et al. 2025（MagNet 同珠 PAC 消化、谱图库、110 min 梯度）：marker 富集 >16 倍，CD9/CD63/SDCBP/NCAM1 >30 倍，ALB 耗竭约 95%，>4,000 蛋白；Suresh & Zhang 2025（MagNet 洗脱后 S-Trap 消化、21 min 梯度、library-free）：785 蛋白，约 69% 与 ExoCarta 相关、Top 100 中 60 个——**两个数字不可直接比较**，梯度、库、消化方式、血浆新鲜程度都不同。

## 常见误用

- "在 ExoCarta/Vesiclepedia Top 100 里"当成验证过的 marker——它只是出现频率高，Vesiclepedia 作者自己在论文里写了这个警告。
- 拿 Vesiclepedia 的 349,988 条蛋白"条目"去比 ExoCarta 的 41,860 个蛋白——前者是研究×蛋白的出现次数，后者是 unique 分子数，口径不同。
- 未证明生物发生就称制备物为"exosomes"。
- 引用 41,860 个蛋白当作 ExoCarta 当前收录量——那是 2016 年数字，且官网统计页已 404；引用 Vesiclepedia 2019 年数字时同样要标年份。
- 用总蛋白量代表 EV 量；或只用总鉴定数判断富集好坏。
- 只用一种表征技术就下结论（MISEV 要求正交证据）。

## 参考文献

- Keerthikumar S et al. ExoCarta: a web-based compendium of exosomal cargo. *J Mol Biol* 2016;428:688–692. doi:10.1016/j.jmb.2015.09.019
- Mathivanan S et al. ExoCarta 2012: database of exosomal proteins, RNA and lipids. *Nucleic Acids Res* 2012;40:D1241–D1244.
- Mathivanan S, Simpson RJ. ExoCarta: a compendium of exosomal proteins and RNA. *Proteomics* 2009;9:4997–5000.
- Kalra H et al. Vesiclepedia: a compendium for extracellular vesicles with continuous community annotation. *PLoS Biol* 2012;10:e1001450. doi:10.1371/journal.pbio.1001450
- Pathan M et al. Vesiclepedia 2019: a compendium of RNA, proteins, lipids and metabolites in extracellular vesicles. *Nucleic Acids Res* 2019;47:D516–D519. doi:10.1093/nar/gky1029 (PMCID PMC6323905)
- Vesiclepedia. http://www.microvesicles.org/ （首页已核查 2026-10-28；无统计数字/更新日期，见上）
- Welsh JA et al. Minimal information for studies of extracellular vesicles (MISEV2023): from basic to advanced approaches. *J Extracell Vesicles* 2024;13:e12404. doi:10.1002/jev2.12404 (PMCID PMC10850029; erratum 2024;13:e12451)
- International Society for Extracellular Vesicles. MISEV. https://www.isev.org/misev
- ExoCarta. http://exocarta.org/ （首页已核查 2026-10-28；统计页 404，见上）
- Wu CC et al. MagNet: *Nat Commun* 2025;16:5447. doi:10.1038/s41467-025-60595-7 （数据 PXD042947）
- Suresh PSS, Zhang Q. Comprehensive comparison of methods for isolation of extracellular vesicles from human plasma. *J Proteome Res* 2025;24:2956–2967. doi:10.1021/acs.jproteome.5c00149 （数据 PXD058948）

*核查记录：MISEV2023 全文（PMC10850029）、ExoCarta 首页、Vesiclepedia 首页与 Pathan et al. 2019 全文（PMC6323905）已于 2026-10-28 逐条核对；ExoCarta 统计页在真实浏览器中返回 404。*
