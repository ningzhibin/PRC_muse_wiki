# 超快低流速 LC-MS：柱后死体积是隐形杀手

**来源**：博客帖 [Post-column dead volume is a massive problem for ultra fast low flow LCMS!](https://proteomicsnews.blogspot.com/2026/10/post-column-dead-volume-is-massive.html)（2026-10-14）；引用的原始论文：*Post-Column Dead Volume as a Critical Determinant in Low-Input, High-Throughput Proteomics*，*Anal. Chem.* 2026，[DOI 10.1021/acs.analchem.6c05567](https://pubs.acs.org/ancham/article-abstract/doi/10.1021/acs.analchem.6c05567/5442681/Post-Column-Dead-Volume-as-a-Critical-Determinant?redirectedFrom=fulltext)（2026-09-30 在线发表）。

## 核心观点

质谱越快，梯度就越敢跑短；梯度一短，那些平时被忽略的"无聊"细节就变成决定成败的因素。柱后死体积（post-column dead volume, PCDV）就是其中最隐蔽的一个——在 5 分钟的 nanoflow 梯度下，仅 emitter 本身的体积就能把峰宽从 ~2.3 s 拉到近 6 s。

## 论文的系统性结论

作者在多种柱内径、流速、梯度长度下，对比了集成柱（integrated emitter）与 union/valve 连接的非集成柱配置：

1. **PCDV 的主要来源**：union 分开连接的 emitter、阀门等柱后连接件。它是 extra-column band broadening（柱外谱带展宽）的主要贡献者之一，此前没有被系统评估过。
2. **峰展宽**：PCDV 显著增加峰宽，**短梯度和低流速下效应最强**——正是超快低流速 proteomics 的工作区间。
3. **覆盖度损失**：PCDV 增大会显著降低蛋白组覆盖度，尤其在**低输入**样品（low-input / single-cell）中。
4. **消除 PCDV 的收益**：用 integrated emitter 消除死体积后，在几乎所有测试条件下**定量精度反而提高**——尽管峰更窄导致每峰数据点更少。在 288 samples/day（5 min sample-to-sample）的高通量平台上，单细胞 proteomics 的覆盖度提高了 **35.8%**。
5. **结论**：对高灵敏度、高通量 nanoLC-MS 分析，最小化柱后死体积至关重要。

## 对 core facility 的实用意义

跑超快梯度之前，先检查柱后连接：integrated emitter（柱-emitter 一体）优于 union 连接的分离式 emitter。这是"换柱"解决不了的问题——问题在柱子**后面**。博客作者的评论很到位：仪器越快，越值得把这种"无聊"的 plumbing 细节当作方法开发的一部分。

*这篇记入 reading-list 的原因是：我们在 Astral 上推超快方法时，LC 的 plumbing 是第一个该怀疑的对象。*
