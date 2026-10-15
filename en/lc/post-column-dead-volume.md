# Post-column dead volume: the hidden killer of ultra-fast low-flow LC-MS

**Source**: blog post [Post-column dead volume is a massive problem for ultra fast low flow LCMS!](https://proteomicsnews.blogspot.com/2026/10/post-column-dead-volume-is-massive.html) (2026-10-14); original paper: *Post-Column Dead Volume as a Critical Determinant in Low-Input, High-Throughput Proteomics*, *Anal. Chem.* 2026, [DOI 10.1021/acs.analchem.6c05567](https://pubs.acs.org/ancham/article-abstract/doi/10.1021/acs.analchem.6c05567/5442681/Post-Column-Dead-Volume-as-a-Critical-Determinant?redirectedFrom=fulltext) (published online 2026-09-30).

## The point

The faster the mass spec, the shorter the gradient you dare to run; and the shorter the gradient, the more those "boring" details nobody thinks about start deciding whether the method works. Post-column dead volume (PCDV) is the sneakiest of them — with a 5-minute nanoflow gradient, the volume of the emitter alone can stretch peak width from ~2.3 s to nearly 6 s.

## The paper's systematic findings

The authors compared integrated-emitter columns against non-integrated configurations connected through unions or valves, across a range of column inner diameters, flow rates, and gradient lengths:

1. **Where PCDV comes from**: union-separated emitters, valves, and other post-column connections — a major contributor to extra-column band broadening that had never been studied systematically across practical configurations.
2. **Peak broadening**: PCDV significantly increases peak widths, with the effect strongest for **short gradients and low flow rates** — exactly the operating regime of ultra-fast low-flow proteomics.
3. **Coverage loss**: higher PCDV dramatically reduces proteome coverage, particularly for **low-input** samples (low-input and single-cell workflows).
4. **Payoff of eliminating it**: integrated emitters, which eliminate PCDV, increased **quantitative precision under virtually all tested conditions** — even though narrower peaks mean fewer data points per peak. On a 288-samples-per-day platform (5 min sample-to-sample), single-cell proteomics coverage improved by **up to 35.8%** relative to a union-connected emitter.
5. **Bottom line**: minimizing post-column dead volume is critical for high-sensitivity, high-throughput nanoLC-MS analyses.

## Practical take for a core facility

Before pushing a faster gradient, check the plumbing *after* the column: an integrated emitter (column + emitter in one piece) beats a union-connected separate emitter. Swapping columns won't fix this — the problem is downstream of the column. The blog author's take is spot on: the faster the instrument, the more these "boring" plumbing details deserve a place in method development.

*Kept in the wiki because: when we push ultra-fast methods on the Astral, the LC plumbing is the first thing to suspect.*
