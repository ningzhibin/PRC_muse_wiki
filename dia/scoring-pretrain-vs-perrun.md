# DIA 打分模型的训练策略：全局预训练 vs 逐文件学习

## 背景

DIA 鉴定里，给 peak group 打分的分类器有两种训练哲学：DIA-NN 式的"每个文件单独学"，和 DIA-BERT 式的"先大规模预训练、再逐文件微调"。这和 FDR 控制里 run-specific vs global 标准的争论是同一个统计学问题。

## DIA-NN：每个 run 单独训练

- 对每个 MS 文件，用该文件自己的 target-decoy 数据现训一个浅层神经网络集成做 peak group 打分
- 输入是手工提取的少量 peak group 特征——这也是它能在 CPU 上跑的原因，代价是信息损失
- 特点：低偏差、高方差。不存在 domain shift 问题（只相信眼前这个文件），但单个文件的 target/decoy 样本有限，分类器天花板低，对低丰度 precursor 尤其吃力

## DIA-BERT：预训练 + 逐文件微调

- 先在 952 个 Orbitrap DIA 文件上预训练一个 transformer 打分模型（仓库里 `resource/model/base.ckpt`）
- 新文件来了，以 `finetune_model.ckpt` 为起点做 file-specific fine-tune；定量另有独立的 `quant.ckpt` 做 peak area 估计
- 本质是 shrinkage / 迁移学习：把跨文件的共享结构（峰形、碎片共洗脱、同位素模式等物理化学规律）先学好

## 和 FDR 的类比

- run-specific FDR：局部校准准，但多文件取并集时整体错误率膨胀
- global FDR：控制实验整体误差，但要求"全局标准"对每个文件都有效
- 打分模型同理：**预训练负责学到通用规律，微调负责对齐当前文件**，两者缺一不可。纯全局模型不敢用（DIA-BERT 自己也要 fine-tune），纯 per-run 则浪费了文件间的共享信息

## 什么时候哪种更好

- 常规 Orbitrap 数据：预训练 + 微调占优（DIA-BERT 自测 precursor +22%、protein +51%，保守 FDR 更低）
- 分布外数据（timsTOF、Astral 等预训练未覆盖的类型）：per-run 策略更稳——模型没见过的数据，带着偏见来不如从零学
- 注意：DIA-BERT 的 benchmark 是作者自测，独立验证还没跟上；预训练的偏见是隐性的（训练集系统性缺失的肽类型学不会）；需要 GPU（推荐 40GB 显存）

## 相关文献

- Liu Z et al. DIA-BERT: pre-trained end-to-end transformer models for enhanced DIA proteomics data analysis. *Nat Commun* 16:3530 (2025). https://doi.org/10.1038/s41467-025-58866-4
- Demichev V et al. DIA-NN: neural networks and interference correction enable deep proteome coverage in high throughput. *Nat Methods* 17:41–44 (2020). https://doi.org/10.1038/s41592-019-0638-x
