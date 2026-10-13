# DIA-BERT 笔记

## 文献

Liu Z, Liu P, Sun Y, et al. DIA-BERT: pre-trained end-to-end transformer models for enhanced DIA proteomics data analysis. *Nat Commun* 16:3530 (2025).
https://doi.org/10.1038/s41467-025-58866-4

代码：https://github.com/guomics-lab/DIA-BERT（学术使用免费，禁止商用）

## 一句话

端到端 transformer 做 DIA 鉴定+定量，不是谱图库生成工具。

## 架构要点

- 952 个 Orbitrap DIA 文件预训练打分模型
- 每个新文件做 file-specific fine-tune
- 定量用独立的 transformer 估计 peak area，训练时部分用了模拟 DIA 数据

## 仓库里的模型文件（`resource/model/`）

| 文件 | 大小 | 用途 |
|---|---|---|
| `base.ckpt` | ~12 MB | 预训练打分模型，鉴定时加载给 peak group 打分 |
| `finetune_model.ckpt` | ~12 MB | 微调起始权重，每个文件 fine-tune 从它开始 |
| `quant.ckpt` | ~11 MB | 定量模型，估计 peak area |

均为 PyTorch Lightning checkpoint 格式。

## 实际限制

- 需要 GPU（推荐 40GB 显存），40GB+ 内存，100GB 硬盘
- v1.0 在 Orbitrap 数据上训练；timsTOF / tripleTOF 作者不推荐
- PTM 和非胰酶切肽未验证
- Astral 数据"未经充分评估"，用 "Other" 仪器类型可跑但需谨慎
- 不支持从 FASTA 直接生成谱图库（可用 DIA-NN 生成 tsv 库再导入）

## Benchmark 用到的库

library-based benchmark 用的是 [DPHL v2](../libraries/dphl-v2.md)（RF 版，601,982 precursors）。
