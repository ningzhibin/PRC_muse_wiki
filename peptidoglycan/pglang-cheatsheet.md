# PGLang 速查：给做蛋白质组学的人

## 文献

Alamán-Zárate MG, Rady BJ, Ledermann R, et al. A software tool and strategy for peptidoglycomics, the high-resolution analysis of bacterial peptidoglycans via LC-MS/MS. *Commun Chem* 8:91 (2025).
https://doi.org/10.1038/s42004-025-01490-6

语法文档：https://github.com/mesnage-org/pgfinder/blob/HEAD/lib/docs/pglang.md
网页工具：https://mesnage-org.github.io/pgfinder/（Mass Calculator、Fragment Generator）

## 一句话

PGLang 就是一条"糖前缀 + 带修饰的肽段序列"，外加描述交联的图语法。做蛋白质组学的人把它当成带糖基化前缀的修饰肽段写法来读就行。

## 1. 基本骨架：`糖链-肽链`

`gm-AEJA` = GlcNAc–MurNAc + Ala–Glu–DAP–Ala

- **糖（小写）**：`g` = GlcNAc，`m` = MurNAc，`x` = 未知糖
- **肽（大写）**：单字母和蛋白质组学一致，**除了五个特殊字母**：`J` = meso-DAP，`B` = 二氨基丁酸，`O` = 鸟氨酸，`U` = 高丝氨酸，`Z` = threo-3-羟基谷氨酸；`X` = 未知氨基酸
- 注意：肽链里的 `E` 是 **γ-D-Glu**（不是 α 连接的）；PGLang 本身不写 L/D，立体化学交给 PGLang→SMILES 翻译器处理

## 2. 修饰：圆括号，紧跟被修饰的残基

`gm(Anh, Glyc)-AEJ(Am)A` —— MurNAc 带 Anh 和 Glyc 修饰，J 带酰胺化；多个修饰逗号分隔。

| 符号 | 名称 | 质量变化 |
|---|---|---|
| Ac | O-乙酰化（6 位羟基） | +42.0106 |
| Am | 酰胺化（侧链羧基） | −0.9840 |
| Anh | 1,6-anhydro（还原端 MurNAc） | −18.0106 |
| DeAc | 去 N-乙酰化 | −42.0106 |
| Glyc | 羟基化（MurNAc 的 N-乙酰 → N-羟乙酰） | +15.9949 |
| Poly | 壁聚合物连接（磷酸） | +77.9507 |
| Red | 还原态（硼氢化钠还原，MurNAc → muramitol） | +2.0157 |

## 3. 侧链：方括号，挂在二氨基/二羧基残基上

`gm-AQK[GGGGG]A` —— 金葡菌的五甘氨酸桥挂在 Lys 上，然后肽链继续 `A`。

Lys 型 + D-Asp 交联桥（Frank Oechslin 项目）按此规则写成 `gm-AEK[D]A` 这类形式（方括号挂在 K 上，里面写 D）。建议粘贴进 Mass Calculator 校验——它会拒绝不合法的挂载位点。

## 4. 多聚体：`=` 连肽链，`~` 连糖链

- `gm-AQK[GGGGG]A=gm-AQK[GGGGG]A (4-3)` —— 交联二聚体，**donor 必须写在前面**
- `gm-AEJA~gm-AEJA` —— 糖链连接的二聚体
- `gm-AEJ=gm-AEJA=gm-AEJAA (3-3, 4-3)` —— 三聚体，交联类型按顺序写在末尾括号里

## 实战流程

Mass Calculator 里粘贴 PGLang → 自动算单同位素质量 → 导出 CSV 直接喂给 MS Analysis 模块；Fragment Generator 则按同样的 PGLang 语法给出每个碎片的归属（signature ion 可用于区分 3-3/4-3 交联异构体、排除质量巧合）。
