# MSFragger 与 Sage：DDA 搜库的速度、结果格式与选型

**整理**：2026-11-05，由当日围绕 MSFragger 的问答讨论整理。起因是 Paulo & Shuken 2026 年 JPR 的 Orbitrap Ascend DIA/DDA 比较论文，其 DDA 条件用的是 MSFragger-DDA+。本文不是对单篇论文的精读，而是把讨论里涉及的问题收在一起：DDA+ 是什么、MSFragger 有多快、结果是什么格式、Sage 与 MSFragger 怎么比、FragPipe 与单独用 MSFragger 怎么选。

## 1. MSFragger-DDA+：把嵌合谱里共碎裂的肽捡回来

传统 DDA 搜库有个默认假设：一张 MS2 谱图只对应一个肽——就是仪器选中的那个前体，只在它前后 5–20 ppm 的窄窗口里找候选。但实际上隔离窗口（通常 0.7–2 Th）里经常同时进来好几个肽，一起碎裂，谱图是嵌合的（chimeric）。那些共碎裂的肽，传统搜库根本不去找，等于白碎了。

嵌合谱这个问题大家早就知道，老办法是先做 MS1 特征检测或谱图去卷积，把共碎裂肽的前体峰找出来再搜。但这要求共碎裂肽在 MS1 里有个高质量的峰——低丰度肽的 MS1 峰常常很弱甚至没有，就漏了。DDA+ 把顺序反过来了：

1. **先搜库，不做 MS1 预处理。** 靠 MSFragger 的碎片离子索引，速度扛得住，直接把整张隔离窗口里所有可能的前体都搜一遍。
2. **再验峰、再打分。** 对每个匹配做靶向 XIC 提取（知道理论 m/z 和电荷后再去 MS1 里定向提峰），用 KL 散度比对同位素分布，扔掉峰形差的；然后贪心算法去掉多个肽共享的碎片，重新打分排序。

顺序一换，低丰度共碎裂肽也能捞到，FDR 还控制得住。效果（Yu, Deng & Nesvizhskii, *Nat Commun* 2025;16:3329）：

- 平均比传统 MSFragger DDA 多鉴定 **57%** 的肽段序列，按酶和碎裂方式不同在 15%（AspN/GluC + CID）到 45%（trypsin + HCD）之间；
- 蛋白定量上，不开 MBR 时多 16–19%，开了 MBR 还多 11–13%；
- 磷酸化上提升小一些，约 5% 的位点、14% 的序列。

还有一个特别之处：它原生支持 **WWA（宽窗口 DDA）**——单细胞蛋白质组里用的那种大隔离窗口采集，谱图跟 DIA 一样多重，传统 DDA 工具处理不了，DIA 工具也不能（因为提不出 XIC）。DDA+ 是少数能直接吃这种数据的搜库模式。

位置：MSFragger 4.0（2023 年 12 月）起内置；论文 2025 年 4 月发表。Paulo & Shuken 的 DDA 条件用的就是 DDA+，其图 S6 显示它比标准 DDA 多鉴定；作者还承认，没有 FAIMS 的时候 DDA+ 的优势会更大（嵌合谱更多）。所以那篇里 DIA 和 DDA 的差距，其实已经是“加强版 DDA”测出来的——换标准 DDA 搜，差距只会更大。

## 2. MSFragger 有多快

核心技巧是碎片离子索引：先把数据库里所有理论肽段的碎片离子建成一张索引，搜库时一张谱图直接查索引匹配，而不是一个个候选肽段慢慢打分。Kong 等 2017 年 *Nat Methods* 的原始论文：比当时主流工具快 100 倍以上。

具体数字（open search，单针 LC-MS/MS，四核工作站）：Comet 13.6 小时、X!Tandem 16.3 小时，MSFragger 5.4 分钟；Tide 在同样约束下 176.7 分钟对 MSFragger 9.8 分钟；SEQUEST HT 在八核机上超过 11 小时。

落到实际使用：常规闭合搜库（窄前体窗口）一针 60–90 分钟的 DDA 在普通台式机上几分钟就完；以前要跑一夜的 open search（±500 Da 找未知修饰）现在也是分钟级。DDA+ 那篇也写了：胰蛋白酶库在 32 GB 内存的主流 CPU 台式机上就能跑，磷酸化或非特异性酶切建议 64–128 GB 内存。

两个提醒：速度吃数据库大小、可变修饰数量和搜库窗口，肽段建索引本身第一次也要几分钟；另外下游的 MSBooster 深度学习重打分和 Percolator 会额外花时间，MSBooster 有 GPU 更快，没有也能跑。跟 MaxQuant（Andromeda）比，同样数据量下 MSFragger 通常快一个数量级以上。

## 3. 结果是什么格式

分两层看：引擎本身和 FragPipe 全流程。

**MSFragger 引擎本身**默认每个谱图文件出一个 pepXML（`<run>.pepXML`）；open search 或 mass offset search 还会额外给一个 TSV（`<run>.tsv`）；其他模式想拿 TSV 的话在 fragger.params 里设 `output_format = tsv_pepXML`。

**FragPipe 全流程**（MSFragger → MSBooster → Percolator → Philosopher → IonQuant）最终拿到的是几个 TSV 报表：

- `psm.tsv`：FDR 过滤后的 PSM，一行一个谱图匹配；
- `ion.tsv`：离子水平（区分电荷态），带定量；
- `peptide.tsv`：肽段水平；
- `protein.tsv`：蛋白水平（多实验或多 plex 时是 `combined_protein.tsv`）。

中间文件还包括 `[filename].pin`（给 Percolator 的输入）、`interact-[filename].pep.xml`、`combined.prot.xml`（protXML 蛋白推断结果）、`protein.fas`（FDR 过滤后的蛋白序列）和 `fragger.params`（搜库参数，可复现）。TMT 的话另有 `tmt-reports/` 文件夹，分基因/蛋白/肽段/修饰位点给丰度和比值；PTM-Shepherd 的修饰分析在 `ptm-shepherd-output/` 里。DIA 走 MSFragger-DIA + DIA-NN 的话，最终是 DIA-NN 的 `report.tsv`/`report.parquet`。

pepXML 是标准格式，要转给别的工具（比如转 mzIdentML）可以用 ProteoWizard 的 idconvert。不过注意 MSFragger 的 pepXML 里有些超出标准 schema 的附加属性（如 `ion_mobility`），个别老工具读的时候可能要留意。

## 4. Sage 与 MSFragger 的比较

**Sage 是什么。** Michael Lazear 用 Rust 写的搜库引擎（*J Proteome Res* 2023;22:3652–3659），算法思路是 MSFragger 碎片离子索引的 clean-room 重实现，但 Rust 的执行效率把速度又推了一截。单个静态二进制，MIT 开源，命令行，只吃 mzML，自带 LFQ 和 TMT 定量、保留时间预测、FDR 控制，为云端和大规模队列设计。

**速度：Sage 更快。** Sage 论文摘要称鉴定性能与现有最好工具相当、速度快一个数量级；作者博客的具体数字：一次典型 TMT 搜库，Sage 比 MSFragger 快约 5 倍、比 Comet 快 20 倍、比 MSGF+ 快 500 倍，且 1% FDR 下 PSM 数相近。第三方也认这个说法：rustims（2025）称 Sage“比已经很快的 MSFragger 还要快一个数量级”；Dear-PSM（2024）实测 Sage 比 MSFragger 省约 5 倍内存。注意 Sage 论文里的 benchmark 是作者自己做的，数字打点折扣看。

**鉴定数：大体相当，但有个 FDR 的坑。** 常规 DDA 上两者鉴定数接近：FastaLake 宏蛋白质组工作里，健康人粪便样本两者肽段数中位比值 1.00。但 MS Andrea（2026，open search）比较发现，用标准 target-decoy FDR 算，Sage 在组蛋白数据集上平均只报 24 个 PSM，而 MSFragger + PeptideProphet 报 418 个；改用 Sage 自己算的 spectrum-level q 值，立刻涨到 677 个；用模型化的后处理统一处理后，三者又回到可比的水平。也就是说，**拿 Sage 跟别人比鉴定数时，FDR 怎么算会极大影响结论**，直接比原始数字会误判。

**特定场景 MSFragger 占优。** 瓜氨酸化稀释系列（*Nat Commun* 2026）里，MSFragger 在各个浓度下都比 MaxQuant 和 Sage 报更多 PSM，原因是它的 labile/hybrid 模式能抓住中性丢失的诊断离子。免疫肽组（*JPR* 2025）同时用了 PEAKS、Comet、Sage、MSFragger 四个引擎：引擎之间一致性很高，真正拉开差距的是 MS2Rescore 重打分，平均多鉴定 27%。

**功能差异：Sage 是轻量单兵，MSFragger 是生态。** Sage 功能窄——open search 只支持有限的可变修饰，没有多修饰组合打分，更没有 PTM-Shepherd 那种修饰谱分析。MSFragger + FragPipe 有 MSBooster 深度学习重打分、Percolator、IonQuant、DDA+、DIA、糖肽、labile 模式全套。

**选型建议。** 搜库速度这件事，两家都已经“解决”了——一针 DDA 都是分钟级，瓶颈不在这儿。日常 DDA 服务样品，Sage 一个二进制文件丢进脚本里就能跑，很适合自动化；但要上 TMT、磷酸化、open search 找未知修饰，或者要 GUI 和完整报表，还是 FragPipe + MSFragger 那套更顺手。DIA 的话两者都不替代 DIA-NN。想试 Sage 注意两点：输入只认 mzML（.raw 要先过 MSConvert），以及 FDR 用它自己的 q 值，别拿它的标准 target-decoy 数直接跟 MSFragger 比。

## 5. FragPipe 还是单独用 MSFragger

结论：绝大多数情况下用 FragPipe。MSFragger 只是搜库引擎，FragPipe 是整条流水线。

**单独用 MSFragger 的局限**——你只拿到流水线的第一步，剩下的都得自己接：

- 没有 FDR 控制：pepXML 是未过滤的，得自己跑 PeptideProphet（TPP）或 Philosopher；想用 Percolator 还得先有 .pin 文件；
- 没有蛋白推断：没有 protXML，没有蛋白分组；
- 没有定量：LFQ 要 IonQuant，TMT 要 TMT-Integrator，SILAC 也一样；
- 没有 MBR，没有 MSBooster 重打分，没有 Crystal-C 质量校准，没有 PTMProphet 定位，没有 PTM-Shepherd，没有 EasyPQP 建库；
- 参数全靠手写 fragger.params，没有预设、没有校验，配错了不容易发现。DDA+ 在 FragPipe 里只是文件类型下拉框里选一下，单独用你得自己知道对应参数；
- 工具管理自己来：MSFragger、Philosopher 这些的下载更新都得手动。

第三种用法是通过 Proteome Discoverer 节点，功能有限。

**FragPipe 不等于只能点鼠标。** 从 18.0 版起有 headless 模式，命令行跑：

```
fragpipe --headless --workflow <workflow 文件> --manifest <manifest 文件> --workdir <结果目录>
```

workflow 与 manifest 文件通常先在 GUI 里配好导出一次，之后就能在服务器或 HPC 上批量跑；也有官方 Docker/Singularity 镜像，还有个叫 Frag'n'Flow 的 Nextflow 封装专门解决大规模部署。也就是说，“把 raw 文件交给工具自动跑搜库”这个想法，FragPipe headless + 一份存好的 workflow 基本就是现成方案，不用自己从零搭。

**单独用 MSFragger 合理的情况**：只要搜库这一步、下游用自己的代码处理 pepXML/TSV；嵌进别的流程里（Galaxy、Nextflow、自建自动化 pipeline）；集群上只想跑引擎本身，不想要整套 GUI 依赖。

**对 PRC 的实际建议**：服务样品的一次性分析走 FragPipe GUI（LFQ-MBR workflow 一把梭：搜库 + FDR + 蛋白推断 + 定量全齐）；要做自动化就用 headless 模式。单独跑 MSFragger 只建议在你明确只要原始 PSM、且下游自己接得住的时候。

**授权提醒**：MSFragger 对学术、非商业、教育用途免费，但 2025 年 5 月 1 日起商业授权转给 Fragmatics, LLC 负责。Core 对外收费服务是否属于“非商业”，使用前值得确认。

## 参考文献

- Kong AT et al. MSFragger: ultrafast and comprehensive peptide identification in mass spectrometry-based proteomics. *Nat Methods*. 2017;14(5):513–520. doi:10.1038/nmeth.4256.
- Yu F, Deng Y, Nesvizhskii AI. MSFragger-DDA+ enhances peptide identification sensitivity with full isolation window search. *Nat Commun*. 2025;16:3329. doi:10.1038/s41467-025-58728-z.
- Lazear MR. Sage: an open-source tool for fast proteomics searching and quantification at scale. *J Proteome Res*. 2023;22(11):3652–3659. doi:10.1021/acs.jproteome.3c00486.
- Paulo JA, Shuken SR. Evaluating data acquisition modes and fragmentation types in label-free proteomics on an Orbitrap Ascend Tribrid mass spectrometer. *J Proteome Res*. 2026. doi:10.1021/acs.jproteome.6c00748.（讨论的起因；其 DDA 条件为 MSFragger-DDA+）
- MSFragger wiki: Interpreting MSFragger Output. https://github.com/Nesvilab/MSFragger/wiki/Interpreting-MSFragger-Output
- FragPipe 文档：Guide to FragPipe results；Running FragPipe in command line interface (headless mode). https://fragpipe.nesvilab.org/
