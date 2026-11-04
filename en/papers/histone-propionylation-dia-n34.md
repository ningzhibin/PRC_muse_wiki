# Deep Read: Histone Propionylation Optimization + DIA Window Scheme Comparison (the Source of N34)

**Source**: Searfoss RM, Karki R, Lin Z, Robison F, Garcia BA (Garcia laboratory, Washington University in St. Louis), *J Am Soc Mass Spectrom* 2024 (PMCID: PMC11017827, PMID: 37853520).

**In one sentence**: The Garcia group tested 8 variables of the histone propionylation workflow one by one (adopting only 2 time-saving ones), and compared 4 DIA window schemes on a Q Exactive, concluding that 34 sequential windows of 24 m/z (N34) is optimal. **The entire meaning of N34 is "34 sequential windows, 24 m/z wide" — there is nothing more mysterious to it.**

## 1. Propionylation Workflow Optimization (8 Variables → 2 Adopted)

Variables tested (one changed at a time, 4 technical replicates, judged by identifications + CV):
1. AmBic 50→100 mM; 2. NH4OH 7→14 µL (more base added); 3. Derivatization 15→10 min; 4. Room temperature→37°C; 5. No drying between the two propionylation rounds; 6. No drying before adding trypsin; 7. Digestion in 2M urea; 8. Digestion at room temperature→37°C.

Results:
- Variables 4 (derivatization at 37°C), 6 (digestion without drying), and 7 (digestion in 2M urea) were **clearly worse** (higher CV, clustering drifted), and were discarded.
- Only variables 2 (more NH4OH to buffer the acidification from propionic acid) + 5 (no drying between rounds) were adopted. Each drying step takes about 3 hours, done twice, so skipping them nearly halves the processing time.
- Correlation between the old and new workflows >0.97, with identifications/CV equal or slightly better.

**Spike-in QC peptide**: GVKFRGSTGGKAPRGKAPATSGMVGPHR (histone-like amino acid composition, but no sequence overlap), added at 1% (0.15 µg / 15 µg histone). The peak-area share of the fully derivatized + fully digested product should be >95%; 7 other products indicate over-propionylation, under-propionylation, incomplete digestion, and oxidation, respectively. A library was built in Skyline for tracking.

## 2. DIA Window Scheme Comparison (the Origin of N34)

Instrument: Q Exactive, MS1 70K (295–1100 m/z, AGC 1E6, max IT 50 ms), MS2 17.5K (fixed first mass 140, AGC 2E5, max IT 60 ms). LC: Vanquish Neo, trap + self-packed 15 cm 3 µm C18, 50 min 2–45% B. Injection 1 µg. Windows generated with the Skyline Isolation Scheme Calculator.

4 schemes (3 injections of the same sample each): N25 = 25 sequential windows × 33 m/z; **N34 = 34 sequential windows × 24 m/z**; S25 = 51 staggered windows × 32 m/z; S34 = 69 staggered windows × 24 m/z.

Results:
- Sequential windows beat staggered windows across the board (identifications + CV); the doubled cycle time of staggered windows is a fatal flaw.
- N34 vs N25: N34 had slightly higher identifications; N25 was slightly better on peak area CV, but **N34 was better on peptide ratio CV (<20% and <5%)** — histone quantification is about ratios, so N34 is recommended.
- Venn: N25 and N34 each had only 13 unique peptides, all extremely low abundance (peak area <3E5, ratio <0.02%). **The two schemes are almost indistinguishable**; N34 is only "slightly better."

Data analysis: EpiProfile 2.1 + human histone PTM library, label-free; manual verification in Skyline.

## 3. Criticisms

1. **N34's "optimal" is a weak optimum**: all 13 unique peptides are at noise level; N25 and N34 make no difference to biological conclusions. N34 is more of a "workflow standardization" choice than a performance breakthrough.
2. **The conclusion is tied to the Q Exactive's speed**: 34 windows × 60 ms max IT + 1 MS1 ≈ 2.5–3 s cycle time, which with a 50 min gradient (peak width about 30 s) gives just about 10 points/peak. Change instruments, and the whole calculation changes. The authors themselves wrote: the optimal scheme depends on instrument cycle time and sample complexity.
3. Only one sample type was used, HEK293T; window effects were not tested on complex samples or low inputs.
4. EpiProfile quantifies only known modification combinations preset in the library; novel PTMs are outside its coverage (an old limitation of bottom-up histone work).

## 4. For the Astral: Is N34 Still Necessary?

**There is no need to copy N34, but the principle of "narrow sequential windows" must be kept.** Three layers:

**(1) The N34 numbers are a product of the Q Exactive era.** The 24 m/z window width is not because histones need 24 m/z; it is because, on the QE at 17.5K and 60 ms IT, 34 windows were the upper limit cycle time could tolerate. The Astral's MS2 speed is about 200 Hz (about 5 ms per spectrum), so 100+ windows still give a cycle time of only about 0.6–1 s. **Copying N34 would mean shackling the Astral with a 2024 QE constraint** — windows can be narrower and cycle time shorter, winning on both ends.

**(2) But "narrower is better" also has a boundary.** Narrower windows → fewer ions per window → lower MS2 signal-to-noise; for low-abundance modifications (precisely the population represented by those 13 "unique peptides") this may instead lose signal. Histone peptides have another characteristic: after propionylation they concentrate in a narrower m/z range with high charge states (mostly z=2–4), so precursor density is high and the benefit of narrow windows is greater than for ordinary whole-proteome work, but the sweet spot needs to be measured empirically.

**(3) Practical recommendations:**
- Do not replicate N34's 24 m/z on the Astral; instead use **narrower sequential windows** within the histone-relevant m/z range (about 300–1100, with actual precursors mostly at 400–900), e.g. 4–8 m/z × 100–150 windows, keeping cycle time within about 1 s (with a 50 min gradient and 15–30 s peak widths, this still gives 15–30 points/peak).
- Alternatively, run the pilot on the Exploris 480 using the original N34, with the Astral method as the upgrade option — running each instrument once at the pilot stage and comparing directly is the most convincing data for a core facility.
- **To be verified**: whether EpiProfile 2.1 supports Astral data. The original paper used 2.1 + the human histone library; whether EpiProfile can directly handle Astral data format/fragmentation characteristics (high scan speed, narrow windows) needs to be checked in newer EpiProfile documentation or tested empirically. The fallback is Skyline + manual verification (the original paper also used Skyline for the spike-in peptide and manual verification).

## 5. One-Sentence Assessment

A solid **method optimization** paper, not a biological discovery. The time-saving propionylation scheme (more base + no drying) and the spike-in QC peptide are immediately usable; the N34 conclusion must be understood under the premise of "QE + 60 ms IT" — **copy the principle (narrow sequential windows, optimize by cycle time), not the numbers**.
