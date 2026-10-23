# ZT Scan DIA 的 1342 Hz 是怎么算出来的：扫描速率的口径问题

SCIEX 给 ZenoTOF 8600 的 ZT Scan DIA 3.0 标称"最高 1342 Hz"，但这个 Hz 和 Orbitrap Astral 的 200 Hz、timsTOF PASEF 的 300 Hz **不是同一个物理量**，不能直接比大小。

## 口径拆解

SCIEX 官网的原话是：四极杆扫描速度最高 **4000 Da/s**，由此可得"MS/MS scan rate up to 1342 Hz"（且注明取决于预设质量范围）。算一下：

$$4000\ \text{Da/s} \div 1342\ \text{Hz} \approx 3\ \text{Da}$$

也就是说 ZT Scan DIA 的 "Hz" = **四极杆滑动速度 ÷ 编码步宽**。Q1 隔离窗口连续滑动，数据流事后按每约 3 Da 切成一个"编码谱"（encoded scan）。这个数字数的是**母离子维度的采样密度**，不是检测器每秒独立积累的谱图数。

对比另外两家，它们的 Hz 是"真谱数"——每张 MS/MS 都有自己独立的注入/积累时间：

- **Orbitrap Astral**：200 Hz（Astral Zoom 270 Hz），每张谱约 4–5 ms 注入
- **Bruker timsTOF（PASEF）**：最高 300 Hz，靠 TIMS 并行累积保住占空比

而 ZT Scan 里一个母离子被窗口扫过的有效时间只有 窗宽 ÷ 扫速：按 2 Da 窗、上限速度算约 **0.5 ms**。

## 厂商自己的应用笔记实际跑多少

| 来源（SCIEX tech note） | Q1 窗宽 | Q1 扫速 | MS/MS 积累 | Cycle（400–900 Da） |
|---|---|---|---|---|
| 蛋白质组 LFQ（ZT Scan DIA 3.0） | 2 Da | 296 Da/s | 6.8 ms | 1.8 s |
| GPF 建库（ZT Scan DIA 3.0） | 1 Da | 101 Da/s | 9.9 ms | ~1.1 s |
| 标称上限 | — | **4000 Da/s** | — | — |

实际方法的扫速只有上限的 1/13 到 1/40，有效谱速在 ~150 Hz 量级。作为参照，同一台仪器的 DDA 模式 MS/MS 约 100 Hz（每张 <10 ms）。

## 那它凭什么还能工作

- **Zeno trap**：碎片离子先在 trap 里囤积、再脉冲进 TOF，占空比 >90%，厂商称 MS/MS 灵敏度提升 5–20 倍——用灵敏度补积累时间不足
- **滑动窗口回链**：碎片可事后关联回近单位分辨率的母离子 bin，选择性优于宽窗 DIA、谱图嵌合少

所以这套方案本身不假，只是它的真实工作点和同行一样落在 **~1–2 s cycle 的窄窗 DIA** 上。

## 横向比"快"，应该比什么

不要比标称 Hz。比这三样，才能看出平台间的真实差异：

1. **Cycle 时间**（扫完一遍母离子范围要多久，决定每个色谱峰能采几个点）
2. **隔离窗宽**（决定谱图嵌合程度与选择性）
3. **每张谱的有效积累时间**（决定离子统计与低丰度肽的下限）

## 参考

- SCIEX ZenoTOF 8600 产品页（"ZT Scan DIA 3.0 supports scan speeds up to 1342 Hz"）：https://sciex.com/products/mass-spectrometers/qtof-systems/8600-system
- SCIEX ZenoTOF 7600+ 产品页（4000 Da/s → 1342 Hz 的推导原文）：https://sciex.com/products/mass-spectrometers/qtof-systems/7600-plus-system
- SCIEX tech note：Narrow-window scanning DIA for unbiased label-free quantitation（ZT Scan DIA 3.0 实际参数，PEAKS 处理）：https://sciex.com/content/dam/SCIEX/pdf/tech-notes/life-science-research/MKT-38229-A-TN_LFQ_Proteomics_Using_Narrow-Window_DIA_20260525-PEAKS.pdf
- SCIEX tech note：Unlocking deeper DIA performance with empirical spectral libraries generated using ZT Scan DIA 3.0（GPF 建库参数）：https://sciex.com/tech-notes/life-science-research/proteomics/unlocking-deeper-dia-performance-with-empirical-spectral-libraries-generated-using-zt-scan-dia-3-0-on-the-zenotof-8600-system
