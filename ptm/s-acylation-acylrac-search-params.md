# S-酰化富集（Acyl-RAC/ABE）不需要特殊的搜库参数

## 一句话

S-酰化富集的化学步骤（羟胺切硫酯键）已经把棕榈酰基去掉了，进质谱的肽段上不带这个修饰——搜库用标准参数即可，不用加棕榈酰化可变修饰。

## 原理：修饰在富集时就被去除了

Acyl-RAC / Acyl-Biotin Exchange 的标准流程：

1. 封闭游离巯基（如 NEM）
2. **羟胺（hydroxylamine, HA）切断硫酯键**——棕榈酸在这里被释放掉
3. 新暴露的巯基被抓到 thiopropyl sepharose 树脂上（Acyl-RAC）或标记生物素后 pulldown（ABE）
4. 还原洗脱（如 BME/DTT）

所以富集到的半胱氨酸是**游离巯基**（洗脱液里是还原态），SP3 流程里还原+烷基化后，全部变成 carbamidomethyl——和普通样品没有区别。

## 搜库参数（文献一致）

已发表的 S-acylome 研究用的都是标准参数：

- 酶：trypsin
- 固定修饰：carbamidomethyl (C)
- 可变修饰：oxidation (M)、acetyl (protein N-term)
- **不加棕榈酰化可变修饰**

依据：Collins/Woodley 的 ssABE 方法学、PPT1 KO 小鼠脑 Acyl-RAC 研究（明确写了"碘乙酰胺烷基化后，原来被棕榈酰化的半胱氨酸以 carbamidomethyl 形式被鉴定"）、neuronal S-acylation 的 Scientific Reports 论文。

## HA- 对照

HA- = minus-hydroxylamine 的阴性对照（不切硫酯键 → S-酰化蛋白不被富集）。搜库参数和 HA+ 完全一样，downstream 做 HA+ vs HA- 的富集比较（t-test + FDR）即可。

## 洗脱缓冲液兼容性

典型的 Acyl-RAC 洗脱液（如 1% BME + 0.2% SDS + 250 mM NaCl + HEPES/TX-100）对 SP3 没有冲突：SP3 本来就是 SDS-compatible，BME 在洗 beads 时去除。

## 真正要注意的

不是修饰参数，而是**物种 FASTA**：DIA-NN library-free 搜库要对物种数据库，"mammalian cells" 定不了 human/mouse/rat——做样前先问清细胞系。

## 相关项目

Dale Martin（UWaterloo）S-酰化 pulldown redo 批次 1501WD1366（2026-10）：16 管到货，只用 8 管标 "PD" 的 pulldown，lysate 放一边；搜库复用上次参数。
