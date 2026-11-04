# 市售 RP 磁珠一览：分泌组富集 + 珠上酶解的选型

**整理**：2026-11-04，由当日分泌组富集讨论整理。价格与供货以厂商最新目录为准；本文只收当时能查到的说明书信息，不是穷尽检索。

## 为什么是 RP 磁珠

分泌组样品的特点是蛋白量少、体积大、基质是培养基。RP 磁珠的结合靠疏水作用，有两个对这场景友好的性质：第一，不怕盐（培养基约 150 mM 盐不干扰结合，甚至有盐析促进作用），不像离子交换磁珠那样需要低盐条件；第二，结合基本不受 pH 影响，pH 8 酶解时蛋白仍然结合在珠子上，是货真价实的珠上酶解——这一点与 SCX（pH 8 释放）和 SAX（pH 8 只留住酸性蛋白）都不同，详见本节另一篇 [在 tip 里还是在 tip 上酶解](sample-prep/in-tip-digestion-geometries.md)。

## 产品

**MagSiproteomics C4 / C8 / C18（Magtivio）。** 磁性硅胶珠，表面分别是 C4、C8、C18 烷基链，10 mg/mL 溶于 25% 乙醇供应，有 2 mL 与 10 mL 两种规格。说明书分工明确：C4 定位为从蛋白混合物中捕获、浓缩、纯化蛋白；C18 定位为肽段脱盐、浓缩（明确写了“分泌到培养基里的肽段”）以及血清/血浆样品的肽段纯化。

**BcMag C-18（Bioclone）。** 约 1 µm 硅胶包铁氧体超顺磁珠，表面 C18 烷基。结合容量 >20 µg 蛋白/mg 珠子，说明书建议 0.5 mg 珠子结合约 10 µg 蛋白。选型建议：C18 适合小分子量蛋白/肽段，低–中分子量蛋白用 C8，大分子量蛋白用 C4。结合条件为 0.1–1% TFA、pH < 4；洗脱用 70% 乙腈。

**Dynabeads RPC 18 / RPC Protein（Thermo）。** 1 µm 单分散磁珠。RPC 18 针对 <5,000 Da 的肽段浓缩与脱盐，RPC Protein 针对 >5,000 Da 的蛋白和大肽段；每 mg 珠子约吸附 14 µg（肽段标准品测得）。同系列当年还有 WCX（弱阳离子交换）与 SAX（强阴离子交换）磁珠——即 SAX 磁珠同样有商品。注意：这是比较老的产品线，下单前务必跟 Thermo 确认现在还在不在售。

## 选型要点

- **链长**：C18 结合最牢，抓得住但完整大蛋白难洗脱；如果走珠上直接酶解，这不要紧，因为最后洗脱的是肽段（50–70% 乙腈很容易下来）；如果中间还要把完整蛋白洗脱下来再酶解，选 C4。
- **容量账**：按 BcMag 的 >20 µg/mg 算，10 mL 培养基里就算只有 10 µg 分泌蛋白，0.5–1 mg 珠子理论上就够了，珠子用量不是瓶颈。
- **结合条件**：RP 结合一般要酸性条件（0.1% TFA，pH < 4），培养基直接加酸调 pH 即可；样品里有机溶剂要先除尽，SDS 等去垢剂要稀释到说明书限值以下。
- **真正的未知数**：多数说明书的定位是肽段脱盐/小体积浓缩，不是“从 mL 级培养基里抓完整分泌蛋白”。稀溶液里的结合动力学（蛋白浓度太低时靠扩散碰到珠子表面需要时间）必须用 pilot 实测，孵育时间要给够。
- **与 SAX 的对比**：SAX 磁珠的结合有 pI 偏倚（只结合 pI 低于缓冲液 pH 的蛋白，碱性分泌蛋白会流穿丢失），且受培养基盐浓度竞争；RP 没有这两个问题，但代价是珠上酶解慢（类似 C18 盘内酶解的扩散限制，过夜是常态）。

## 产品链接

- [MagSiproteomics C4 / C8 / C18 说明书（Magtivio，经 AMSBIO）](https://resources.amsbio.com/Datasheets/MD01009.pdf)
- [BcMag C-18 Magnetic Beads 说明书（Bioclone）](https://www.bioclone.us/files/BcMag_C-18_Magnetic_Beads.pdf)
- [Magnetic Bead Chromatography 宣传册（Thermo，含 Dynabeads RPC 18 / RPC Protein / WCX / SAX）](https://assets.thermofisher.com/TFS-Assets/LSG/brochures/Magnetic_Bead_Chromatography.pdf)
