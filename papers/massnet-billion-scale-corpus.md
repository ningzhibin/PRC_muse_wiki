# 文献精读：MassNet —— 十亿级 AI-ready 质谱语料库

**来源**：Guo 团队（西湖大学 Tiannan Guo、复旦/上海 AI 实验室 Siqi Sun、上海 AI 实验室 Zhiqiang Gao 通讯；共同一作 Jun A.、Xiang Zhang、Xiaofan Zhang、Jiaqi Wei——即同时维护 DIA-BERT 的 Guomics 团队），*Nature Methods* 2026-10-14 在线发表，DOI: [10.1038/s41592-026-03247-y](https://www.nature.com/articles/s41592-026-03247-y)（OA, CC BY-NC-ND 4.0）。

**一句话**：把 648 个项目、27,643 个 LC-MS/MS run、15.4 亿张 DDA 二级谱图整理成统一张量格式（MSDT），训练出 de novo 测序模型 XuanjiNovo；自称蛋白质组学的"ImageNet"，但标签是搜索引擎伪标签、且 DDA-only、无未鉴定谱图。

## 1. 论文结构与核心声明：数字对账

- **~30 TB** = 原始数据（648 个项目，来自 PRIDE + iProX，**27,643** 个 LC-MS/MS run，**35 物种**，10+ 高分辨平台，**DDA-only**）；
- **15.4 亿** = 全部 MS/MS 谱图数；
- **5.48 亿 PSMs** = FragPipe（4.48 亿）与 Sage（3.51 亿）各自 1% FDR 鉴定、合并去重后的 run-level 数；**4.45 TB** 是其 MSDT 存储体积；全局 FDR 后剩 **5.01 亿** PSMs / 204 万 unique peptides / 122,056 蛋白。
- **关键缺口：仅 ~36% 谱图被鉴定保留**，约 64% 未鉴定谱图被丢弃——而这恰恰是 de novo 最有价值的部分（作者自承牺牲了复杂/边缘肽段的覆盖）。

## 2. MSDT 张量格式

**MSDT** = 基于 **Apache Parquet** 列式存储的 schema：每行一个谱图–鉴定对，含 scan 号、前体 m/z/电荷、RT、对齐的 fragment **m/z–intensity 数组**、肽段/蛋白映射、target-decoy 标签、搜索引擎打分，另附 CSV 元数据（仪器、碎裂方式、碰撞能量、project ID、软件版本、物种）。

**为何"张量"降门槛**：传统 MGF/mzML 与 .msp/.sptxt/.tsv 分离、无统一 schema、文本行导向；MSDT 用 schema 定义的数值数组，天然支持 batching、并行、对接 PyTorch/TF。实测 7 种格式后选 Parquet（压缩–IO 折中；csv.gz 压缩比最高 ~5.89× 但读慢 ~7.88s）；run-level 文件比源数据小 **~82%**。务实细节：Parquet 利存储/顺序扫描而不利训练随机访问，故训练时转 **LMDB**（存储与训练解耦；Casanovo 亦从 HDF5 迁至 Lance）。

**QC**：双引擎独立搜库、run-level FDR，每谱只留最低 PEP 的 PSM，全局 peptide/protein FDR 1%（PGA / picked_group_fdr）；训练集另要求前体质量误差 <0.1 Da，训练/验证按肽段序列无重叠划分。

**对比既往资源**：MassIVE-KB（人类为主、强溯源）、ProteomeTools（合成人类库）被指三短板——偏人类、峰列表与鉴定分离、非 AI-ready；MassNet 保留每个 run-level 谱图完整峰列表而非 consensus 谱。

## 3. 学习范式：课程学习与"训练崩溃"（机制深读）

**"训练崩溃"** = 在 MassNet 级异质数据上，NAT 模型**高达 80% 的训练运行无法收敛**（早期 loss 不降/震荡/发散）。

**成因（两层相乘）**：① NAT 并行独立预测所有 token，假设空间指数膨胀（自回归则逐 token 收缩空间）；② CTC 使每条 label 对应 **O(nT) 条对齐路径**（blank/重复折叠），远超 cross-entropy 的单一路经。论文形式化为 **SPACE(NAT) ~ γ × TYPE(Loss) × ρ_ratio**，TYPE(CTC) >> TYPE(CE)。35 物种×10+ 平台的分布噪声在早期淹没梯度信号 → 优化失败。

**解法（CTC 路径采样三步）**：① 空输入前向，枚举折叠为真值 A 的有效路径，选**概率最高的一条** ŷ（必须在路径而非 label 上 mask，因 CTC 路径与 label 无一一对应）；② 按比例 ρ 随机 mask ŷ，得 ρ(A,ŷ) 作 decoder 条件输入；③ 仍用**原始 CTC 目标**训练（最大化所有有效路径总概率），防死记单条路径。**难度调度**：ρ=0 全揭示（任务平凡）→ ρ→1 全 mask（纯生成）；实现 peek = max(**0.93** − epoch×**0.01**, 0)。**推理**：全 mask 起始 → argmax 得 y⁰ → 以 yⁱ⁻¹ 为条件迭代精炼（默认 3 次）→ **PMC**（0-1 背包变体，动态规划在前体质量容差内找最大 log-prob 序列）。

**直觉类比**（对 Karpathy 路线读者）：把"从零生成"先降级为"BERT 式填空"——早期给 93% 答案（含最可能的对齐结构，隐式引导 attention），只补少量空，再逐步撤提示。精妙处在于**监督信号动态自举**（每步选当前最可能路径），形成 EM 式循环。

## 4. XuanjiNovo 模型

**架构**：谱图编码器（m/z 正弦编码 + intensity 嵌入 → transformer）→ NAT 解码器（双向自注意力 + 对谱图交叉注意力，每位置独立 softmax）→ PMC 质量约束。基于 π-PrimeNovo 的 NAT+CTC 框架。

**训练**：**1 亿 PSMs**（仅 FragPipe 来源），MSDT→LMDB。

**Benchmark**（15 物种自建集：307 raw 文件、3078 万谱图、966 万 PSMs，含 Orbitrap Astral；基线 π-HelixNovo / Casanovo V2 / InstaNovo+ / ContraNovo / π-PrimeNovo）：peptide recall human **0.45**（π-PrimeNovo 0.32 / InstaNovo+ 0.26）、mouse **0.68**（0.60/0.52）；K/Q 近等质量判别 **0.78/0.71**（基线 0.71/0.68）；scaling 30M→65M→100M：氨基酸 recall 0.75→0.78→**0.82**，peptide recall 0.54→**0.68**（π-PrimeNovo ~0.30）。**但是**：经典 nine-species 基准上 100M 版反而不如现有方法（QE vs QE HF+、PEAKS 8.0 vs FragPipe 21.1 的分布差）；用 3000 万 MassIVE-KB 微调得 **XuanjiNovo-130M** 后反超（酵母 +64.62%、human 0.60 略低于 ContraNovo 0.62）。HCC 集 0.68 vs 0.38（+77%）。

**"SOTA"准确含义**：自建 15 物种基准全物种领先；跨仪器/标注管线需微调才有竞争力——SOTA 有条件。

## 5. 数据/代码可用性（已核验）

- **PRIDE PXD071590**：论文声明已存储（53,023 run-level MSDT、全局 FDR 结果、1 亿训练集、15 物种 benchmark）。⚠️ 本次未能独立验证可访问性（项目页面抓取超时）；旁证：MassNet-DDA issue #10（2026-02-25）曾有用户报告无法访问，3-11 被关闭。Zenodo 备份：10.5281/zenodo.17538551、10.5281/zenodo.21703577。
- **代码**：`github.com/guomics-lab/MassNet-DDA`（XuanjiNovo 代码+权重 `XuanjiNovo_100M_massnet.ckpt` / `_130M_massnet_massivekb.ckpt`，Google Drive/HF 镜像+SHA256；Docker `guomics2017/massnet-dda:cuda11_v1.1`/`cuda12_v1.1`）。
- **MSDT-Converter**：`github.com/guomics-lab/MSDT-Converter`（mzML/Bruker .d/MGF/WIFF→MSDT 及逆转换；Docker `guomics2017/msdt-converter`）。
- **"xuanjinovo-novotax"**：**非官方**，是第三方 fork（`mateuslab-prot/xuanjinovo-novotax`，为 NovoTax 管线改）。
- **许可证**：论文称 Apache 2.0；**已验** MassNet-DDA 根 LICENSE 文件正文确为 Apache 2.0（GitHub 页显 "NOASSERTION" 仅分类器未识别）。MSDT-Converter 含 LICENSE.md 与 "MIT License.md"（未逐字验）。

## 6. 批判性评估

- **DDA-only**：DIA/靶向明确列为未来工作；DIA 的 multiplexed 谱图与 DDA 结构根本不同，对 Astral DIA 用户无直接覆盖。
- **未鉴定谱图缺口**：de novo 核心价值在解析未知谱，但 ~64% 未鉴定谱被丢弃（作者自承牺牲复杂/边缘肽段覆盖）——内在张力最大。
- **物种不平衡**：human/mouse 主导；human recall 0.45 远低于酵母 0.69。
- **标注质量**：标签 = 搜索引擎伪标签；双引擎打分/FDR 策略差异引入系统偏差；训练集仅用 FragPipe 来源。
- **PTM 窄**：仅 Met 氧化、N 端乙酰化（可变）+ Cys 氨基甲酰甲基化（固定）；无磷酸化/泛素化/糖基化。
- **"ImageNet"半实半虚**：实——规模/标准化/AI-ready 是里程碑；虚——ImageNet 是人工真值，MassNet 标签是带噪伪标签，且仅验证 de novo/RT/重打分三任务。

## 7. 对 core facility 的相关性

- **(a) 在 Astral DDA 数据上跑 XuanjiNovo**：可行。Docker 开箱即用（需 Linux + NVIDIA GPU，CuPy 依赖，macOS 不行）；输入 MGF（raw→mzML→MGF 或 MSDT-Converter）；约 **280 spectra/GPU**（H200）。注意训练集无 Astral 数据、无罕见修饰，human benchmark recall 0.45——可用，别期望数据库搜索精度。
- **(b) 与 DIA-BERT 的关系**：任务不同（de novo 生成 vs DIA peak-group 打分），共享 transformer 谱图编码、双向上下文、大规模训练思想；且 MassNet 的重打分演示（MassNet_4M）用的正是 **DDA-BERT 架构**——同一团队 DDA/DIA 双线并行，学通 NAT+CTC 对理解 DIA-BERT 的谱图表示有直接帮助。
- **(c) 宏蛋白质组/库搜索**：MassNet 按单物种 Swiss-Prot 建库，直接迁移有限；MSDT 格式与课程学习范式可复用。遗憾的是 de novo 对宏蛋白质组最有价值的场景（未知谱图）正是被丢弃的部分；第三方 NovoTax fork 表明已有人尝试接入分类学管线，值得跟踪。

## 核心要点

1. **数字**：15.4 亿谱（~30 TB）→ 5.48 亿 PSM（4.45 TB）→ 全局 FDR 后 5.01 亿；仅 ~36% 谱图保留，未鉴定谱是最大缺口。
2. **最值得学的是 CTC 路径采样课程学习**："93% 答案填空、每 epoch 撤 1%"把 NAT 指数级对齐搜索空间逐步打开，将 80% 训练失败变为稳定收敛——NAT+CTC 在噪声异质数据上的通用技巧。
3. **XuanjiNovo 自建基准 SOTA 但跨分布需微调**；Astral 零训练数据却表现好，然 DIA 完全不在覆盖范围。
4. **代码/权重 Apache 2.0（已验 LICENSE），Docker 开箱即用**；PRIDE PXD071590 本次未能独立验证可访问（曾有故障 issue）。
5. **"ImageNet"半实半虚**：规模标准化是真的，但伪标签、DDA-only、无未鉴定谱——对 DIA/宏蛋白质组用户更多是方法学参考。
