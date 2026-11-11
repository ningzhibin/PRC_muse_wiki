# 文献精读：MetaUmbra — 从宏蛋白质组肽段做基因组水平存在性推断

**来源**：Wu Q, Ning Z, Zhang A, Cheng K, Figeys D. MetaUmbra: Statistically Controlled Genome-Level Presence Inference from Metaproteomic Peptides. bioRxiv 2026.04.29.721689**v2**（DOI: 10.64898/2026.04.29.721689）。v2 修订说明：更新了统计框架与基准分析，新增 Monte Carlo 设置敏感性分析与 leave-one-reference-out 实验。预印本，未经同行评审。代码：https://github.com/byemaxx/MetaUmbra（BSD-3-Clause）。

**一句话**：MetaUmbra 不做丰度、不做差异、不做功能，只回答一个问题——给定一个用户自定义的基因组 panel 和一张鉴定到的肽段表，panel 里每个基因组在样品中存在还是不存在，并给出带多重检验校正的 p 值和 q 值；核心是把肽段证据拆成 unique 与 shared 两路，shared 肽不扔掉、按 degeneracy 加权后用 Monte Carlo 经验 null 检验，unique 肽用背景基因组校准，两路证据经 Simes 交集检验 + closed testing 合并，且**没有 unique 肽的基因组永远得不到显著结论**。

## 1. 要解决的问题

- 宏蛋白质组里大量肽段来自保守蛋白、同时属于多个基因组，肽段→基因组的映射天生含糊。现有工具的缺口：Unipept 用 LCA（lowest common ancestor）把含糊证据上推到高阶分类单元，稳健但丢分辨率，且推断时不用肽段分数；MetaLab 等蛋白中心流程先做蛋白推断再注释分类，引入蛋白分组的假设与偏倚；Peptonizer2000 给 taxon 水平的贝叶斯置信分、MiCId 给物种/属水平的统计置信度，但都不对**用户自定义基因组 panel**给出基因组水平的 p 值和 q 值。
- MetaUmbra 的定位就是补这个缺口：肽段中心（peptide-centric）、基因组水平、带正式统计检验，适用于自定义菌株 panel、MAG 目录、宿主–微生物混合参考，或迭代精炼的 follow-up panel。推断结果永远相对于 panel 的组成而言。

## 2. 工作流总览：两步，digest 只做一次

1. **建库（reference construction）**：每个基因组一个蛋白 FASTA 文件，RPG v2.0.5 做 in silico 酶解，生成每个基因组的理论肽段参考表。同一基因组集合 + 同一酶解设置只需做一次。
2. **打分（genome presence scoring）**：输入观测肽段表（MaxQuant 或 DIA-NN 的肽段表），与理论参考匹配，在统计框架内评估，输出每个基因组的 p 值、BH 校正 q 值和 presence score。

工具形态：GUI（`metaumbra-gui`）、命令行（`metaumbra digest` / `metaumbra score` / `metaumbra extract-parquet`，后者把 DIA-NN parquet 报告转成肽段 TSV 再打分）、Python 函数式 workflow；已集成进 MetaX v2.0 的肽段注释流程（Wu et al., *Anal Chem* 2025;97:9739–9747）。Python 3.10+，`pip install metaumbra[all]`。

## 3. 理论肽段参考库：先看清含糊的全貌

- **酶解设置**（基准分析所用，需与上游鉴定流程一致）：Trypsin/P，肽段长度 7–30 aa，≤2 个漏切。保留的肽段按基因组导出为肽段参考表，肽段→蛋白→基因组的链接保留下来，这是计算 degeneracy 的基础。
- **I/L 等价匹配**：质谱一般分不开异亮氨酸与亮氨酸，主分析把 I 和 L 都映射为 `J` 再去重、匹配、算 degeneracy；精确序列匹配只作敏感性分析。Q/K、AD/W 不合并。
- **参考空间有多含糊**（UHGG 4,744 个代表基因组，in silico 酶解后）：**3.841 亿条非冗余理论肽段，其中 84.3% 是某个基因组独有的，15.7% 被至少两个基因组共享**。共享主要集中在低 degeneracy：2–5 个基因组共享的占全部理论肽的 13.9%，6–20 占 1.52%，>20 仅 0.21%，101–500 占 0.036%，>500 占 0.004%。含糊是实质性的，但主要是局部的、不是全局的。
- **基因组间理论肽数差异很大**：每个基因组非冗余理论肽数中位数 108,482 条，分布右偏很重——所以原始匹配肽数不能直接跨基因组比较，必须校准。这就是 unique 证据要引入"理论 unique 肽机会数"（见 §4.3）的原因。
- SIHUMIx 肽段集对全 UHGG 背景匹配时，全部 4,746 个候选基因组都至少匹配到一条肽——只看匹配数完全不够。

## 4. 打分机制（核心）

### 4.1 预处理：分数只是相对权重

- 用 decoy 与肽段误差字段（PEP 等）先过滤鉴定；I/L 归一化后去重，重复序列只保留证据分数最高的一条；保留的分数做 min-max 归一到 [0,1]；没有可用分数时 s(p) = 1。
- **关键**：这些分数只作相对证据权重用，不是鉴定概率；肽段误差度量与 unique 证据模型里的固定 α = 0.05 是分开的。

### 4.2  degeneracy 加权：shared 肽不扔，按含糊程度打折

- 对基因组 k，τ_k 为其理论肽段集，观测肽段集记为 O，匹配集 M_k = O ∩ τ_k。
- 每条观测肽 p 的 **degeneracy d(p)** = panel 里含有 p 的基因组数。权重 w(p) = 1/d(p)：unique 肽（d = 1）权重 1，shared 肽按 degeneracy 打折。基因组 k 的加权证据 E_k = Σ_{p ∈ M_k} w(p)·s(p)。
- shared 证据 = d(p) > 1 的匹配肽；unique 肽 = d(p) = 1。

### 4.3 Shared 证据：肽段空间 Monte Carlo 经验 null

- 把观测 shared 肽的加权贡献 w(p)·s(p) 按 degeneracy 分 bin 汇集：**2–5、6–20、21–100、101–500、>500**（bin 边界未针对基准结果优化）。
- 对每个基因组，按它匹配到的 shared 肽在各 bin 的计数，从这些池子里抽样，生成该基因组特异的 null 分布；经验 p 值 p_S = (1 + #{Monte Carlo 复本的 shared 证据 ≥ 观测值}) / (1 + B)，B 为复本数。
- 复本数：首轮 500（seed 1）；近阈值精修：Mix24X 单菌株、近属 pooled、仓鼠分析用 20,000，SIHUMIx 与全 UHGG pooled Mix24X 用 200,000。
- 意义：检验"观测到的 shared 支持是否超过了仅由肽段含糊性带来的期望"，**不需要单独建一个 decoy 基因组数据库**。

### 4.4 Unique 证据：两种 null 模型，自动选择

- 先算每个基因组的**理论 unique 肽机会数 A_k**：在当前 panel 内只属于该基因组的理论肽段数（归一化后、打分前算）。
- **empirical-background 模型**（有足够可比背景时）：把观测 unique 肽数 U_k 与 A_k 相近的弱背景基因组比较。候选基因组本身与最高支持的候选者排除在背景之外；统计量用背景 unique 肽数的 95th 分位 Q_0.95,k，α = 0.05，数值下限 1e-300。背景排除是迭代的：初始排除比例 0.10，每轮按 unique 肽数排序排除最高支持的一部分，重新算统计量、合并、BH 校正；下一轮排除比例 = 中间 q ≤ 0.20 的匹配基因组占比，界在 [0.05, 0.20]，变化 < 0.01 或 5 轮后停止。中间 q 只用于背景估计，不定义正式的 presence call。
- **alpha-upper-bound 模型**（没有足够可比背景时）：乘法模型，把 α 当作保留的 unique 肽误差贡献的上界。**正文明确：它依赖肽段独立性与误差界假设，不应解释为基因组存在的后验概率。**
- **自动选择规则**：按理论机会分层，要求 ≥100 个可比基因组、期望上尾计数 ≥5、且 ≥90% 的候选者有足够背景，才用 empirical-background，否则整个 panel 用 alpha-upper-bound。选择不用 pilot p/q 值、不用基准标签、不用回收率。SIHUMIx 大 panel 与全 UHGG Mix24X 用 empirical-background；24 基因组的单菌株 Mix24X 与 73 基因组的近属 panel 用 alpha-upper-bound；仓鼠分析明确指定 alpha-upper-bound。
- README 的说法与此一致：默认 `auto` 模式，先按理论肽段机会分区，检查 ≥90% 候选者是否各有 ≥100 个可比背景基因组且背景分位上期望观测 ≥5；符合则 empirical-background，否则全 panel 用 alpha-upper-bound；决策变量与原因记在 `artifacts/diagnostics/unit_empirical_background_calibration.tsv`。

### 4.5 合并：Simes 交集检验 + closed testing，unique 是门槛

- unique 证据 p_U 与 shared 证据 p_S 来自同一观测肽段集与同一 panel，独立性无法保证，所以 Fisher 合并（要求分量 p 值独立）**只保留作敏感性分析**。
- 主分析：先对 p_U、p_S 做**两分量 Simes 交集检验**，再对 unique 分量做 **closed testing**，得到最终基因组水平 p 值 p_G。README 给出的公式：

  `p_G = max(p_U, min(1, 2·min(p_U, p_S), max(p_U, p_S)))`

- **U = 0（没有观测到 unique 肽）时 p_G = 1**。因此 p_G 不可能小于 p_U，**shared 证据再强也不能单独确立正式的 presence call**。
- 每个独立分析的肽段集与候选 panel 内，对至少匹配到一条观测肽的基因组做 **BH 校正**，得 q 值。主阈值 q ≤ 0.01，次要 q ≤ 0.05。presence score 是基于 q 值的可视化排序分（正文公式以图给出）。Harmonic-mean、Bonferroni、unique-only 也保留作敏感性比较。

## 5. 基准测试结果

### 5.1 SIHUMIx：8/8 找回，零多余

- Panel 4,746 个基因组（8 个预期 + 4,738 个 UHGG 背景，预先去掉了 6 个与 SIHUMIx 成员同种的背景基因组）。**8 个预期基因组全部找回，背景中零多余显著基因组**，q ≤ 0.01 与 q ≤ 0.05 均如此。预期成员原始 p 值 2.0×10⁻³⁰⁰ 至 3.13×10⁻⁸，q 值 3.164×10⁻²⁹⁷ 至 1.854×10⁻⁵。
- 排序很紧凑：前 8 个基因组占累计匹配肽覆盖的 99.8%。但 unique/shared 结构差异极大：*Lactobacillus plantarum* 仅 6 条 unique / 189 条 shared，*Clostridium butyricum* 25/441，而 *Erysipelatoclostridium ramosum* 2,986/3,360、*Bacteroides thetaiotaomicron* 5,057/23,879。**弱预期成员（6 条 unique）仍显著，而只有 shared 证据的背景候选者在 U=0 规则下被挡住**——这正是 §4.5 的设计在起作用。

### 5.2 Mix24X：24/24 找回，难的是近邻

- **单菌株分析**（24 套肽段各对 24 基因组 panel）：每套都找回预期基因组；20 套零多余，4 套各多一个近缘基因组（*Bacillus cereus*/*B. thuringiensis*、*Salmonella bongori*/*Shigella flexneri* 两对）。
- **pooled 分析**（24 套肽段合并）：近属 panel（73 基因组）与全 UHGG panel（4,764 基因组）里 24 个预期基因组都排在前 24 名内；最大排名移动是 *Bordetella parapertussis* 从第 5 滑到第 16。近属 panel 多 3 个显著基因组，全 UHGG panel 只多 1 个——**扩大背景没有增加多余 call**，但它改变了肽段的 unique/shared 归属（24 个预期基因组的 unique 肽比例中位数从 61.41% 降到 56.43%，中位每基因组少 83.5 条 unique 归属）与适用的 unique 证据模型（近属用 alpha-upper-bound，全 UHGG 用 empirical-background）。难的基因组仍保住大部分 unique 支持：*B. cereus* 153 条里留 148 条，*B. thuringiensis* 49 条留 47 条，*S. flexneri* 134 条留 117 条。

### 5.3 与 Unipept、Peptonizer2000 的比较（描述性，非等价基准）

- 外部工具的结果是修订前保留的旧输出、未重跑，所以只作回收率与输出紧凑度的描述性比较。
- SIHUMIx：Unipept 8/8 + 5 个多余 taxon；Peptonizer2000 共报 26 个 taxon，在推荐的 0.8 置信阈值下丢了 *C. butyricum* 与 *L. plantarum*；MetaUmbra 8/8、零多余。
- Mix24X：Unipept 24/24，共 39 个 taxon；Peptonizer2000 共 36 个 taxon、找回 23/24，0.8 阈值下丢了 *B. parapertussis*、*B. cereus*、*B. thuringiensis*、*P. putida*、*S. flexneri*、*Cellulophaga lytica*；MetaUmbra 两 pooled panel 均为 24/24，显著基因组分别为 27（近属）与 25（全 UHGG）个。
- 注意：taxon 水平与基因组水平的输出推断目标不同，不可直接等同比较；"多余报告"按基准成员身份定义，不是 BH FDR 的估计。

### 5.4 仓鼠肠道 MAG panel：真实数据里的证据结构

- 706 个 GTDB-Tk 注释的细菌 MAG（90 个 shotgun 宏基因组共组装分箱得 926 个 bin，保留 706 个）。没有封闭的预期集，所以**不是回收基准**，只看证据结构。
- I/L 等价匹配 + alpha-upper-bound：**587/706 个 MAG 在 q ≤ 0.01 与 q ≤ 0.05 下均显著**；精确序列匹配 588 个显著，两者共享 578 个（Jaccard 0.9682），说明结论基本不受 I/L 归一化影响，少数基因组的 call 依赖于它。
- 强支持 MAG 包括 *Escherichia coli*、*Desulfovibrio* sp. 及 Bacteroidaceae、Muribaculaceae、Lachnospiraceae、Bifidobacteriaceae 相关 MAG。因为很多候选者打到相同的最小 q 值，正式显著性无法在组内排序，作者另定义了一个**描述性 display score**（标准化 log unique 肽数 + 标准化 shared 证据显著性之和）仅用于可视化排序。

### 5.5 补充评估

- 另有两个已发表混合数据集的补充评估（Supplementary Fig. S3/S4）：六菌株培养混合物（Valdés-Mas et al., *Cell* 2025；PXD057701）与 ATCC 20 菌株 SPEED 标准品（PXD011189）。

## 6. 稳健性：三个值得记住的数字

- **没有显著 call 只靠 0 或 1 条 unique 肽**：1–3 条 unique 肽的低支持类别占 SIHUMIx 0/8、Mix24X 近属 pooled 2/27、全 UHGG pooled 0/25、24 个单菌株分析 1/28——这三条低支持基准记录全是多余 call、且各有 3 条 unique 肽。仓鼠 panel 里 71/587 个显著 MAG 落在低支持类别。
- **Monte Carlo 设置不敏感**：3 个基准 panel × 3 种 bin 方案 × 3 个随机种子（1、7、42）共 27 种组合，显著 call 集与默认分析完全相同（Jaccard = 1）。
- **Leave-one-reference-out：参考不完整会造出假 call**。删掉一个预期参考后，剩下的预期基因组全部保留（SIHUMIx 每次 7/7，Mix24X 每次 23/23），但在 q ≤ 0.01 下凭空多出 **73 个 SIHUMIx 与 59 个 Mix24X 新显著事件**。机制一致：总匹配肽数不变，原先与被删参考共享的肽变成了另一个候选者的 panel-unique 肽；132 个新显著事件里 panel-unique 计数上升、shared 计数相应下降，117 个候选者在基线分析里本来一条 panel-unique 肽都没有。序列层面核对：9,512 条 SIHUMIx 与 1,334 条 Mix24X 肽段–事件记录在删参考后获得唯一性，且每条序列原来恰好只匹配新 call 的基因组与被删参考。
- **计算成本**：打分 73 基因组 panel 用 43.6 秒 / 峰值 2.35 GiB；4,764 基因组全 UHGG panel 约 18 分钟 / 47 GiB。建库（酶解）单独测：在笔记本 16 进程下 100/200/500 个 UHGP FASTA 分别 69.7/139.5/345.9 秒，峰值内存 <0.44 GiB。敏感性分析用 MetaUmbra 1.5.0。

## 7. 局限与使用边界（Discussion 的原意）

- **推断以 panel 为条件**：panel-unique 支持不是普遍的物种特异性；显著基因组要与合理的近缘竞争者、参考完整性一起解读。近缘基因组间的区分仍受肽段共享限制，作者建议从宽 panel 向密集采样的种/株水平参考迭代精炼。
- **不要用固定 unique 肽数阈值**（如 ">3 条 unique 肽才算"）：肽段在小 panel 里是 unique 的，扩大背景或加入近缘基因组后可能就不再 unique；固定阈值在不同参考空间下不稳定。MetaUmbra 的正式框架正是要替代这种做法。
- **其他局限**：依赖上游肽段鉴定质量、分数校准与酶解假设；DIA 数据的碎片水平依赖性没有显式建模。MetaUmbra 是更 broad 的宏蛋白质组流程里的**基因组存在性模块**，本身不估计丰度、不做差异或功能分析。
- **数据**：SIHUMIx PXD023217（DDA，MaxQuant 2.6.7.0）；Mix24X PXD005728（DDA，Mascot 2.5.1）；仓鼠 DIA-PASEF PXD057158（DIA-NN 1.9.1）；仓鼠 MAG 参考 Zenodo 13909289。资助：NSERC RGPIN-03905-2018。

## 8. 对小 panel 应用的启示

- MetaUmbra 的设计里最难简化的部分是 null：1–3 个基因组的 panel 没有可比背景，自动走的就是 **alpha-upper-bound** 模型（24/73 基因组 panel 的先例），且它明确声明不是存在后验概率。
- 另一条不可简化的硬规则：**U = 0 时 p = 1**。任何简化版工具如果去掉这条，shared 肽 alone 就能"证明"存在，整个框架的保守性就没了。
- 输入只需要一张肽段表（DIA-NN 或 MaxQuant），不依赖搜库本身；但打分质量完全取决于上游鉴定与酶解设置是否一致（Trypsin/P、7–30 aa、≤2 漏切）。

## 9. 输出文件

- `unit_genome_results.tsv`：每个分析单元 × 基因组一行，含 q 值与阈值标志；`cohort_genome_summary.tsv`：跨分析单元的复现情况；`sample_unit_mapping.tsv`：样本归属；`genome_selection_manifest.json`：唯一的下游基因组选择接口（每个单元 q ≤ 0.05 的基因组与更严的 q ≤ 0.01 子集）；`artifacts/`：运行元数据、日志、缓存与可选诊断表。关键列：`num_peptides_matched`、`num_peptides_unique`、`theoretical_unique_peptides`、`expected_unique_null`、`unique_depth_fold`（观测 unique / null 期望）、`pvalue_shared`、`pvalue_unique`、`pvalue_simes_intersection`、`pvalue_simes_closed`（默认的 `pvalue`）、`qvalue`、`presence_score`。
