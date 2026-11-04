# 文献精读：Suresh & Zhang 2025 — 9 种血浆 EV 分离方法全面比较

**来源**：Patil Shivprasad Suresh, Qibin Zhang（通讯；UNC Greensboro, Center for Translational Biomedical Research / Dept. of Chemistry & Biochemistry），*J Proteome Res* 2025, 24(6):2956–2967（DOI: 10.1021/acs.jproteome.5c00149；PMID: 40356199；PMC: PMC12150312；CC-BY 4.0）。数据 PRIDE **PXD058948**；资助 NIDDK R01 DK114345。

**一句话**：这篇的价值不在“MagNet 赢了”，而在它把 MagNet 的 785 个蛋白放在一个**非 on-bead 消化、21 分钟梯度、library-free** 的统一流程里测出来——这是洗脱样品的合理参照，而不是 Wu et al. 2025 的 4,000+ 上限。两者差的不是珠子，是整个下游流程。

## 1. 研究设计：一句话

同一个 pooled 血浆、每种方法 100 µL、三重复，9 种方法（4 类原理）单方法分离后，用同一套下游（NTA + Simple Western + bottom-up DIA 蛋白质组）比较产量、粒径分布、纯度和蛋白覆盖。作者自称是首个用 100 µL 血浆做 9 种方法详细表征的报告。组合方法（DGUC+SEC 等）被有意排除，理由是临床小体积样本经不起多步损失。

- **样本**：健康供者 EDTA 血浆，来自功能食品干预运动应激恢复的临床试验（NCT03445234），2000*g* 10 min 取血浆后 pooled、−80°C 冻存；用前室温解冻、3000*g* 10 min 去碎片。**冻存 pooled 血浆**——这是全文最大的外部效度限制（见 §6）。
- **统一归一化**：所有方法分离出的 EV 都经 10 kDa Amicon 浓缩/换液到 100 µL PBS，去除残留聚合物、盐和分离试剂。这个步骤让下游比较相对公平，但也意味着：此文的 MagNet 结果**不等于** Wu 的 MagNet 全流程结果（见 §2 末尾）。
- **9 种方法**：离心类 UC、DGUC；SEC 类 qEVsingle 35 nm、SmartSEC；聚合物沉淀类 ExoQuick ULTRA、Total Exosome Isolation（TEI）；静电/亲和类 MagNet（MagReSyn SAX）、Norgen NPR（SiC 树脂）、MagCap（Tim4-PS 亲和）。

## 2. 九种方法的具体做法（只记关键参数和 MagNet 的分叉点）

- **UC**：100 µL 稀释到 11.9 mL PBS，150,000*g* 3 h，沉淀重悬再 120,000*g* 3 h。耗时最长的一档。
- **DGUC**：UC 沉淀铺到 5/10/20/40% iodixanol 梯度，120,000*g* 18 h，取上层 6 mL，再 120,000*g* 4 h。密度分离，纯度换产量。
- **qEV**：qEVsingle 35 nm，0.7 mL void 弃掉，收接下来 0.7 mL。
- **SmartSEC**：100 µL + 400 µL PBS 上柱，室温旋转 30 min，500*g* 30 s 洗脱。
- **ExoQuick ULTRA**：100 µL + 30 µL 沉淀剂，冰上 30 min，3000*g* 10 min，再过 kit 的纯化柱（Buffer B/A）。
- **TEI**：100 µL + 50 µL PBS + 20 µL 沉淀剂，室温 10 min，10,000*g* 5 min。
- **NPR**（Norgen）：100 µL 稀释到 3.9 mL 水 + ExoC，加 Slurry E 树脂，ExoR 洗脱、mini-filter 柱收样。
- **MagNet**（关键）：MagReSyn SAX（MR-SAX002），50 mM Bis-Tris pH 6.5 + 150 mM NaCl 平衡；血浆 1:1 混 100 mM Bis-Tris Propane pH 6.3 + 150 mM NaCl，室温 30 min 结合，洗 3 次。**然后洗脱**：100 µL 洗脱液（25 mM Bis-Tris Propane pH 6.5、1 M NaCl、0.1% Tween 20），洗脱液再走 10 kDa 换液。**此文的 MagNet 是“分离用 SAX 珠子、洗脱后换 S-Trap 消化”，不是 Wu/厂家手册的同一批珠子 on-bead PAC 消化。** 这是解读 785 这个数字时最重要的一句。
- **MagCap**：Tim4-PS 亲和，血浆先加 5 U 肝素降非特异结合，4°C 1 h，洗 3 次，50 µL 洗脱液洗 2 次。只抓 PS+ EV，亚群最窄。

## 3. 表征与蛋白质组方法（统一的下游，决定了数字的可比边界）

- **NTA**：ZetaView Quatt 散射模式，1000 倍稀释，每样三重复。注意：散射模式数的是所有颗粒，脂蛋白也算数，产量数字不能直接读成 EV 产量。
- **Simple Western**（Jess 全自动）：三重复各取 20 µL pooled 后测；另对全部 27 个样单独测 CD9 做重复性。marker：CD9、CD81、Alix、annexin A2、HSPA8、TSG101；污染物：albumin、ApoA1、ApoE3。RIPA 裂解 + DTT、95°C。图 3A 的上样按**同一起始血浆体积**归一化。
- **蛋白质组**：S-Trap 消化。取相当于 3 µL 血浆的 EV（补到 50 µL PBS）+ 50 µL 2× 裂解液（10% SDS、100 mM TEAB），超声，TCEP 55°C 还原，IAA 烷基化，磷酸酸化，90% 甲醇结合液上 S-Trap micro 柱，trypsin/LysC 1 µg、47°C 3 h。EvoTip 上样，Evosep One（8 cm × 150 µm、1.5 µm C18），1 µL/min，**21 分钟梯度（60 SPD）**，Orbitrap Ascend：MS1 380–985 m/z、60k，DIA 10 m/z 窗口、30k，NCE 25。
- **搜库**：DIA-NN 1.8.1 **library-free**，UniProt 人库（2024-06-07 下载），Trypsin/P、1 个漏切，Cys 固定 carbamidomethyl，MBR 开启，基因水平推断。Perseus 1.6.14：>70% 有效值过滤、log2、宽度归一化、正态分布插补、permutation FDR <0.05、S0 <0.05。>70% 过滤 + 插补意味着低丰度蛋白先被筛掉一批，数字偏保守。

## 4. 结果

### 4.1 NTA：粒径与产量——选择性与产量成反比

- 平均粒径 90–117 nm。分布最窄：MagCap、NPR、MagNet、DGUC；最宽：TEI、ExoQ、qEV、SSEC、UC，>200 nm 颗粒占比可观。
- <30 nm 非 EV 颗粒（脂蛋白/exomere/蛋白聚集体）：ExoQ 约 4%、TEI 约 2%，其余 <1%。
- 产量：选择性方法（MagCap、MagNet、NPR、DGUC）每 100 µL 血浆 1.4–4.7×10⁹ 颗粒，比 UC/qEV/SSEC（1.2–8.4×10¹⁰）低 8–17 倍。qEV 产量最高但约 15% 是大 EV，SSEC 约 17%。UC 1.2×10¹⁰ vs DGUC 3.5×10⁹；TEI 1.8×10¹⁰ 略高于 ExoQ 1.4×10¹⁰（ExoQ 多了柱纯化一步）。
- 所有方法粒径和产量的 CV <20%。作者的结论句：选择性越高，产量越低，群体越均一。

### 4.2 Simple Western：纯度

- 9 种方法相对 neat plasma 都富集了 EV marker。CD81 只在 NPR、MagNet、MagCap 检出（Discussion 里又多了 ExoQ——**正文前后不一致**，见 §6），且信号远弱于 CD9；作者推测与用冻存血浆有关（冻存降低 CD81）。
- 同体积上样下 marker 信号最强：qEV、SSEC、ExoQ、MagNet、MagCap。
- 污染物：albumin 在 UC、TEI、NPR 最高；DGUC 去脂蛋白最彻底，且 ApoE3 的去除好于 ApoA1——密度解释：ApoE3 所在的 LDL/VLDL（1.006–1.063 g/mL）与 EV（1.10–1.21）密度差大，ApoA1 的 HDL（1.063–1.21）与 EV 重叠，难分开。
- CD9 重复性：ExoQ、TEI、MagNet 信号高但 CV 约 18–29%；紧接着又写 MagCap 和 MagNet CV <7%——**MagNet 同时出现在两句里，前后矛盾**，不要引用这组 CV 作结论。NTA 的 CV（<20%）是更干净的重复性证据。

### 4.3 蛋白质组：MagNet 785 最高，但要看条件

- 鉴定数（library-free、21 min）：**MagNet 785、MagCap 778 最高**；NPR 最低 570；对照 neat plasma 634、HansaBioMed 血浆 EV 标准品 723、细胞 EV 标准品 767。全方法 >570。
- 除 NPR（23%）外各方法蛋白覆盖 CV <20%。PCA 按方法聚类（PC1+PC2 44.4%），方法内三重复几乎无批次效应；neat plasma 和标准品沿 PC1 明显分开。
- 相对 neat plasma 的火山图（原文倍数写法含糊，写的是“~7-fold, log2 scale”，按原文数字记，不换算）：CD9 在 ExoQ/MagNet/MagCap 富集最高（约 7 倍），TSG101 在 qEV/TEI/MagNet 最高（约 6 倍）；albumin 去除 DGUC 最好（约 5 倍）、qEV 约 4 倍、其余约 3 倍；ApoA1 各方法约 2–3 倍去除；ApoE DGUC/NPR 约 4 倍、其余约 2 倍；ApoB100 在 UC/DGUC 约 10 倍、其余约 6 倍。
- ExoCarta 对照：9 种方法 EV 相关蛋白占比都 >63%；MagNet 约 69%、MagCap 约 68%，其中 60 个属于 ExoCarta top 100 marker。方法特异蛋白：MagCap 独有 28 个（12 个与超分子纤维组织、8 个与细胞投射调控相关，作者联想到 PS+ EV 与细胞骨架/运动）；MagNet 独有 20 个（中性粒细胞脱颗粒、氨基酸生物合成相关）。作者自己也只做到功能联想一级，没有验证。

## 5. Discussion 里最关键的一段：785 为什么不能和 Wu 的 4000+ 比

作者明说：本研究的工作流是为通量和无偏比较优化的，不是为最大覆盖优化的。三个变量叠加决定数字：

1. **梯度**：21 分钟。作者未发表数据：梯度翻倍，MagNet 785 → 1080。
2. **建库**：library-free。Wu 用谱图库 + 95 分钟梯度，>4000 蛋白。
3. **消化方式**：此文是洗脱后 S-Trap 消化，**没有用 MagNet on-bead 消化**。作者未发表数据：完整 on-bead MagNet protocol、同样 21 分钟梯度、library-free，1219 个蛋白（比 785 高约 55%）。
4. **血浆新鲜度**：作者说这是影响最大的因素。新鲜血浆 + 完整 on-bead protocol、21 分钟、library-free，**2492 个蛋白**（未发表数据）。

也就是说：785（冻存、洗脱、S-Trap、21 min、library-free）→ 1219（on-bead）→ 2492（on-bead + 新鲜血浆），再往上才是 Wu 的谱图库 + 95 分钟梯度的 4000+。**这四个数字是同一条阶梯，不是四个互相矛盾的结论。** 引用时必须带上条件，只报 785 或只报 4000 都是误导。

## 6. 局限与批评（作者自述 + 我的）

作者自述：① pooled 健康供者血浆，疾病状态/个体差异（年龄、性别、代谢）无法外推；② 冻存血浆可能引入聚集、改变表面 marker（如 CD81 下降）；③ 污染物只查了 albumin 和载脂蛋白，非囊泡颗粒没有充分评估。

我的补充：

- **“MagNet”在此文只是分离步骤的标签**。珠子抓完就洗脱了，后面和别的 8 种方法走同一套 S-Trap 流程。所以这篇能回答“哪种分离法在统一消化下最干净”，不能回答“Wu 全流程能挖多深”。洗脱送样、短梯度、library-free 的样品，785 才是它的参照系。
- **蛋白组上样按起始血浆体积归一化**（相当于 3 µL 血浆的 EV），不是按蛋白量或颗粒数归一化。产量低的方法天然吃亏一部分，覆盖数里混着产量效应和纯度效应，分不开。
- **重复性是技术重复**：同一 pooled 血浆分三份，CV <20% 证明的是流程稳定，不是生物学重复。疾病队列里个体差异会大得多。
- **正文两处数字前后不一致**：CD81 的方法名单（Results vs Discussion）、MagNet 的 CD9 CV（18–29% vs <7%）。不影响主结论，但引用细节时要以 Results/图为准，或干脆不引。
- **没有成本/时间表**：Discussion 说选方法时考虑了样本量、成本、时间、设备和重复性，但全文没有一张成本/耗时对比表；也没有 TEM。DGUC 的 18 h + 4 h 在临床通量场景下基本不可行，这一点只在结论里一笔带过。
- **倍数表述不严谨**：火山图倍数写成“~7-fold, log2 scale”，log2 倍数和倍数混用，无法还原成统一尺度，本文只按原文转述。

## 7. 对实际应用的意义

- **MagNet 的选择站得住**：在统一流程的 9 方法比较里，MagNet 和 MagCap 并列纯度最高、粒径分布最窄、覆盖最高，代价是产量低 8–17 倍。对生物标志物发现（小体积、要纯度）这个取舍是对的；对要产量的场景则不是。
- **洗脱样品的判断标尺**：如果样品是洗脱送样、短梯度、library-free，鉴定数落在 785 附近并不意味着富集失败；要看 CD9/TSG101 富集和 albumin/ApoB 去除，而不是总数。总数要和 Wu 的 4000+ 比，必须先把 on-bead 消化、长梯度、谱图库、新鲜血浆四个条件补齐。
- **下一步最划算的三个改动（按作者未发表数据的幅度排序）**：① 回到 on-bead PAC 消化（785 → 1219，+55%，同一梯度同一搜库）；② 新鲜血浆（→ 2492，幅度最大，但临床样本不一定做得到）；③ 长梯度 + 谱图库（785 → 1080 只是梯度翻倍的效果，建库的增益在 Wu 的数据里）。三者叠加才有 4000+ 的量级，单改一项到不了。
- **MagCap 是备选，不是替代**：覆盖和 MagNet 相当，但 Tim4 只抓 PS+ EV，抓的是更窄的亚群；MagNet 的静电吸附谱更宽。换方法等于换生物学问题，比较前要先定亚群定义。
- **DGUC 不用追**：去污染最好，但 22 小时两轮超速离心 + 产量最低，不适合临床样本量的项目。

## 8. 引用与核查

- 全文 PMC 免费阅读（CC-BY 4.0），本文数字均核对全文，非摘要转述。
- 补充材料：Simple Western 全图（Fig. S1）、聚类图（Fig. S2）、各方法 DIA-NN 鉴定蛋白表（Table S1，xlsx），均未在本篇精读中逐项核对；如需方法特异蛋白名单，应直接查 Table S1 / PRIDE PXD058948。
