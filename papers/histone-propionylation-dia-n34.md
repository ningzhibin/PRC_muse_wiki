# 文献精读：组蛋白丙酰化优化 + DIA 窗口方案比较（N34 的出处）

**来源**：Searfoss RM, Karki R, Lin Z, Robison F, Garcia BA（Garcia 实验室，Washington University in St. Louis），*J Am Soc Mass Spectrom* 2024（PMCID: PMC11017827，PMID: 37853520）。

**一句话**：Garcia 组把组蛋白丙酰化流程的 8 个变量逐一测试（只采纳 2 个省时的），又在 Q Exactive 上比较了 4 种 DIA 窗口方案，结论是 34 个 24 m/z 顺序窗口（N34）最优。**N34 的全部含义就是“34 个 24 m/z 宽的顺序窗口”，没有别的玄机。**

## 1. 丙酰化流程优化（8 变量 → 采纳 2 个）

测试变量（每次只动一个，4 技术重复，看鉴定数 + CV）：
1. AmBic 50→100 mM；2. NH4OH 7→14 µL（多加碱）；3. 衍生 15→10 min；4. 室温→37°C；5. 两轮丙酰化之间不烘干；6. 加 trypsin 前不烘干；7. 酶解用 2M urea；8. 酶解室温→37°C。

结果：
- 变量 4（37°C 衍生）、6（不烘干直接酶解）、7（2M urea 酶解）**明显变差**（CV 升高、聚类偏离），弃用。
- 只采纳变量 2（多加 NH4OH 缓冲丙酸酸化）+ 变量 5（两轮之间不烘干）。烘干每次约 3 小时、做两次，省掉后处理时间几乎减半。
- 新旧流程相关性 >0.97，鉴定数/CV 持平或略好。

**Spike-in 质控肽**：GVKFRGSTGGKAPRGKAPATSGMVGPHR（类组蛋白氨基酸组成、但无序列重叠），按 1%（0.15 µg / 15 µg 组蛋白）加入。衍生 + 酶解完全的产物峰面积占比应 >95%；另有 7 种产物分别指示过丙酰化、欠丙酰化、不完全酶解、氧化。用 Skyline 建库跟踪。

## 2. DIA 窗口方案比较（N34 的由来）

仪器：Q Exactive，MS1 70K（295–1100 m/z，AGC 1E6，max IT 50 ms），MS2 17.5K（fixed first mass 140，AGC 2E5，max IT 60 ms）。LC：Vanquish Neo，trap + 自装 15 cm 3 µm C18，50 min 2–45% B。进样 1 µg。窗口用 Skyline Isolation Scheme Calculator 生成。

4 种方案（同一样品各 3 针）：N25 = 顺序 25 窗口 × 33 m/z；**N34 = 顺序 34 窗口 × 24 m/z**；S25 = 交错 51 窗口 × 32 m/z；S34 = 交错 69 窗口 × 24 m/z。

结果：
- 顺序窗口全面优于交错窗口（鉴定数 + CV），交错窗口 cycle time 翻倍是硬伤。
- N34 vs N25：N34 鉴定数略高；peak area CV 上 N25 略好，但 **peptide ratio CV（<20% 和 <5%）上 N34 更好**——组蛋白定量看的是 ratio，所以推荐 N34。
- Venn：N25/N34 各自独有的肽只有 13 条，且全是极低丰度（peak area <3E5，ratio <0.02%）。**两种方案几乎没差别**，N34 只是“略胜”。

数据分析：EpiProfile 2.1 + 人组蛋白 PTM 库，label-free；Skyline 手工验证。

## 3. 批评

1. **N34 的“最优”是弱最优**：13 条独有肽全是噪音水平，N25 和 N34 在生物学结论上无差别。N34 更多的是一种“流程标准化”的选择，而非性能断层。
2. **结论绑定在 Q Exactive 的速度上**：34 窗口 × 60 ms max IT + 1 个 MS1 ≈ 2.5–3 s cycle time，配 50 min 梯度（峰宽约 30 s）刚好约 10 个点/峰。换仪器，算式全变。作者自己也写了：最优方案取决于仪器 cycle time 和样品复杂度。
3. 只用了 HEK293T 一种样品类型；没测复杂样品/低输入下的窗口效应。
4. EpiProfile 定量的是预设 library 里的已知修饰组合，novel PTM 不在覆盖范围（bottom-up 组蛋白的老局限）。

## 4. 对 Astral：还有必要 N34 吗？

**没必要照搬 N34，但“窄顺序窗口”这个原则要保留。** 分三层说：

**（1）N34 的数字是 Q Exactive 时代的产物。** 24 m/z 窗宽不是因为组蛋白需要 24 m/z，而是 QE 在 17.5K、60 ms IT 下，34 个窗口是 cycle time 能容忍的上限。Astral 的 MS2 速度约 200 Hz（每谱约 5 ms），做 100+ 个窗口 cycle time 也就约 0.6–1 s。**照搬 N34 等于用 2024 年 QE 的枷锁去束缚 Astral**——窗口可以更窄、cycle time 反而更短，两头都赢。

**（2）但“越窄越好”也有边界。** 窗口收窄 → 每个窗口离子数减少 → MS2 信噪比下降；对低丰度修饰（正是那 13 条“独有肽”代表的群体）可能反而丢信号。组蛋白肽段还有个特点：丙酰化后集中在较窄的 m/z 范围、电荷态高（z=2–4 为主），前体密度大，窄窗口的收益比普通全蛋白组更大，但 sweet spot 需要实测。

**（3）实操建议：**
- 不要在 Astral 上复刻 N34 的 24 m/z，而是在组蛋白相关的 m/z 范围（约 300–1100，实际前体多在 400–900）内做**更窄的顺序窗口**，比如 4–8 m/z × 100–150 个窗口，cycle time 控制在约 1 s 以内（50 min 梯度下峰宽 15–30 s，仍有 15–30 个点/峰）。
- 或者先用 Exploris 480 按原文 N34 跑 pilot，Astral 方法作为升级选项——pilot 阶段两种仪器各跑一次，直接对比，对 core facility 是最有说服力的数据。
- **待核实**：EpiProfile 2.1 是否支持 Astral 数据。原文用的是 2.1 + 人组蛋白库；Astral 的数据格式/碎裂特性（高扫描速度、窄窗口）EpiProfile 能否直接吃，需要查新版 EpiProfile 的说明或实测。备选是 Skyline + 手工验证（原文也用了 Skyline 做 spike-in 肽和手工验证）。

## 5. 一句话评价

一篇扎实的**方法优化**文章，不是 biological discovery。丙酰化省时方案（多加碱 + 不烘干）和 spike-in 质控肽是拿来即用的干货；N34 的结论要放在“QE + 60 ms IT”的前提下理解——**抄原则（窄顺序窗口、按 cycle time 优化），不抄数字**。
