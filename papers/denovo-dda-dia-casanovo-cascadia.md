# 文献精读：DDA / DIA de novo 测序现状 — Casanovo 系与 Cascadia

**来源**：2026-10-22 由一条 CasanovoGUI 新闻引发的追问链，顺带把 DIA de novo 的两条线（第三方 Casanovo-DIA vs Noble 本家 Cascadia）核清。涉及文献见文末。

**一句话**：DDA de novo 目前证据最硬的是 Casanovo，DIA de novo 要试就用 Cascadia（唯一在 Astral 4 Th 窄窗上原生评测过的）；两者都只是候选生成，不能单独作鉴定证据。

## 1. DDA 侧：Casanovo 本体 + CasanovoGUI

**GUI 论文本身没有新 benchmark**——它是软件说明文：把 `casanovo sequence / db-search / train` 做成 Java 桌面标签页，首次启动自动装私有 Python 环境（按 GPU 匹配 build），结果在 View 里看注释谱图（PDV）、逐残基置信度、肽段回贴参考蛋白组（mismatch-tolerant mapping，但 GUI 文档没给 mismatch 上限、也没有 mapping 专项验证）。版本/权重/config 对齐时 GUI 与 CLI 结果应一致，**GUI 不产生准确率加成**。

效果数字只能回到模型论文，且注意指标多为 precision–coverage / AP，**AP 0.95 ≠ 95% 肽段全对**：

- 九物种 benchmark（ICML 2022，leave-one-species-out）：precursor 过滤后 coverage 0.53–0.68 时，肽段 precision **0.67–0.82**、AA precision 0.90–0.95（DeepNovo 肽段只有 0.25–0.46）；强迫 coverage=1 时肽段 precision 掉到 0.37–0.56——高分一部分来自放弃低置信谱
- *Nat Commun* 2024：九物种训练 AP 0.83；换 MassIVE-KB 约 3000 万 PSM 训练 → 肽段 AP **0.95**、AA AP 0.98；≥4+ 电荷 AP 只降约 12%（竞品 >60%）
- 独立抗体评测（2023，用老 v3.0.0）：recall 与 AA precision 参评最好；+ALPS 组装轻链覆盖 97.7–99.5%、重链仅 76.4–93.7%——肽段级强、全长组装难
- 速度（官方 Performance 页，Casanovo 5.2.1 + `casanovo_orbitrap_v5-2-0.ckpt`，Eclipse HeLa DDA 86,311 张谱）：GPU **530 s（163 谱/s）** vs CPU 10,926 s；显存限 3.5 GiB 也能跑完。**注意版本断层：v5.2 这个 checkpoint 没有公开的准确率 benchmark，上面的 AP 数字是更早世代的**

诚实局限（跨全节通用）：

- **胰酶偏倚有实测**：MassIVE-KB 训练集全是 trypsin；非酶切留出集上标准版 AP 0.60、专门微调的 Casanovo_ne 0.83。非胰酶场景要用微调版或先本地验证
- **PTM 词表有限**（2024 模型：28 token）——M 氧化、脱酰胺、N 端乙酰等有；**磷酸化、糖基化、Lys 乙酰/甲基/丙酰化不在词表，输出不了**
- 长肽（>18 AA）明显变差；I/L 等同质量歧义是物理上限；ETD/EAD 无系统 benchmark
- score 是模型概率类分数，**不是 FDR/q 值**——de novo 的严格置信度控制至今未解决

## 2. “Casanovo-DIA” — 第三方的同名工作，不是 Noble 做的

- Ebrahimi & Guo（University of North Texas），IEEE BIBE 会议论文级研究原型；在 Casanovo transformer 上加 encoder block 融合 MS1/MS2/precursor profile
- 在 DeepNovo-DIA 的 3 个人源 DIA 数据集上赢 DeepNovo-DIA 与 PepNet（AA 级 precision +15–35%，肽段级 +59–81%）
- 代码 github.com/Biocomputing-Research-Group/Casanovo-DIA（GPL）；**未并入官方 Casanovo，GUI 里也没有**。引用时注意别把它当 Noble 实验室的产品线

## 3. DIA 侧正主：Cascadia（Noble 实验室，*Nat Methods* 2025）

**核心设计 — augmented spectrum**：DIA 里一条肽的证据散在多张谱里、且无准 precursor。Cascadia 不做 DIA-Umpire 伪谱、也不做 MS1 feature 检测：以一张 MS2 为中心，打包同窗口前后各 2 张 MS2（共 5 张）+ 对应 5 张 MS1 的同位素峰，每峰带 (m/z, 强度, 时间偏移, MS1/MS2 标记)，喂改造版 Casanovo transformer；推理时对 run 内每张谱都预测，precursor 由模型自己从 MS1 信号推。

- 为什么这条路线对：DIA-NN 鉴定的肽中能找到 DIA-Umpire MS1 feature 的比例，宽窗 Lumos 71.4%、**窄窗 Astral 仅 47.2%**——伪谱路线在 Astral 数据上先天丢一半
- 训练：encoder 先在 MassIVE-KB DDA 数据预训练（**消融：跳过预训练直接训 DIA 数据不收敛**），再加 b/y 离子分类辅助 loss；宽窗模型在 CPTAC 372 个 run、约 5M 肽段上微调；窄窗模型另在 77 个小鼠血浆 EV（Mag-Net）样本的 878,217 个 augmented spectra 上训
- 成绩（肽段级，按回贴蛋白组估 precision）：
  - 宽窗 CPTAC（Lumos，16 Th）：90% precision 下 **2,371 条** vs DeepNovo-DIA 529；Casanovo 跑原谱 29 条、跑伪谱 64 条
  - **窄窗 Astral（4 Th）**：90% precision 时 2,137 → **6,224 条**；score 校准良好、可当概率卡阈值
  - 正交验证：皮肤样本 159 个候选 SNV、21 个被外显子测序证实（AUC 0.92），DeepNovo-DIA 57 个候选 0 证实；血浆 EV 中 3,305 条 Ig 候选肽、47.7% 被 DIA-NN 回搜支持（DeepNovo-DIA 32%）
- 短板：**慢**——一个 Astral run 在 A100 上约 5 h（DIA-NN 同数据 CPU <30 min）；GitHub 无正式 release、PyPI 文档自称 WIP；胰酶偏倚仍在（作者建议按应用微调）；修饰肽训练数据不足、PTM 预测/定位未评测；timsTOF 淌度维度未用；评估的 precision 只是下界（回贴不到 ≠ 错）
- 输入 mzML，输出 mzTab / .ssl（可进 Skyline 当谱图库）；Apache 2.0，github.com/Noble-Lab/cascadia

## 4. 对 core facility 的结论

- DDA de novo：Casanovo 是目前证据最硬的，定位 = tryptic 高分辨 DDA 的候选生成（宏蛋白质组未解释谱、抗体、数据库不全样本）；单独作鉴定证据不行，要 score 阈值 + 回贴 + 看谱复核，关键序列合成肽验证
- DIA de novo：要试就用 **Cascadia**（唯一在 Astral 4 Th 窄窗上原生评测过的），定位是 DIA-NN 的 complement、专挖数据库外的新肽，正对宏蛋白质组/未研究生物场景；成本主要是 GPU 机时（约 5 h/run）
- Astral DDA 上 Casanovo 没有任何专项 benchmark（官方测速用 Eclipse），数字不能直接外推

**涉及文献**：Wen B et al. CasanovoGUI. *J Proteome Res* 2026（DOI: 10.1021/acs.jproteome.6c00617）；Yilmaz M et al. Casanovo 模型论文：ICML 2022；*Nat Commun* 2024（DOI: 10.1038/s41467-024-49731-x）；Ebrahimi S, Guo X. Casanovo-DIA. IEEE BIBE 2024 / arXiv:2402.11363（PMC11044815）；Sanders J, Wen B, et al. Cascadia. *Nat Methods* 2025, 22:1447–1453（DOI: 10.1038/s41592-025-02718-y）；旁证：Beslic et al. 抗体 de novo 工具独立评测，*Brief Bioinform* 2023。
