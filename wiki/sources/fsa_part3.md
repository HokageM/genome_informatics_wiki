---
title: FSA Part 3
type: source
module: GSA
units: [3]
sources: [fsa_part3]
tags: [gsa, source, pssm, msa, phylogeny, linear-space, exam-relevant]
status: solid
updated: 2026-10-21
---

# FSA Part 3

Raw file: `raw/foundations_sequence_analysis_part3.pdf`, 30 handwritten pages.

## Scope

Part 3 of *Grundlagen der Sequenzanalyse* covers five blocks:

| pp. | Block |
|---|---|
| 1–2 | Position specific scoring matrices and the PSSM searching problem, look-ahead scoring |
| 3–5 | Addenda to parts 1–2: LCS length formula, swap edit operation, partitions, q-gram code, multi-eops lists, `qgdist` vs `edist` |
| 6–9 | Significance of local alignments: Hamming distance, E-values, expected counts of maximal exact matches and of k-LMAs |
| 10–14 | Linear-space alignment: column sweep, crosspoints, `R_{j_0}`, `C`, divide and conquer |
| 15–24 | Multiple sequence alignment: definition, consensus and SP costs, k-dimensional DP, star alignment, progressive/iterative alignment, D&C alignment |
| 25–30 | Phylogenetic analysis: trees, additivity, four-point condition, UPGMA, ultrametrics, Neighbor Joining |

No numbered exercises (Aufgaben) appear in part 3; all worked items are lecture examples.

## Key takeaways

- A sequence family is summarised by a [[position-specific-scoring-matrix]]
  `M : A × {1..m} → ℝ` and scored additively by `score(w,M) = Σ_h M(w[h],h)` (fsa_part3, p. 1).
- The naive sliding-window PSSM search costs `O(mn)`; [[pssm-lookahead-search]] aborts early with
  intermediate thresholds `th_d = th − σ_d` and runs in `O(kn)` on average (fsa_part3, p. 2).
- [[alignment-significance]]: the E-value is the *expected number* of equally good or better local
  alignments between random sequences; `E[#maximal exact matches of length ≥ ℓ] = m·n·p^ℓ(1−p)`
  (fsa_part3, pp. 6–9).
- [[hirschberg-linear-space-alignment]] recovers one optimal alignment in `O(m+n)` space and
  `O(mn)` time by recursing on the middle column `j_0 = ⌊n/2⌋` and the crosspoint `(i_0, j_0)`
  (fsa_part3, pp. 10–14).
- [[multiple-sequence-alignment]] is scored either by **consensus cost** (`O(k)` per column) or by
  **sum of pairs** (`O(k²)` per column) (fsa_part3, pp. 15–17).
- [[multidimensional-dp-msa]] solves the MSA exactly in `O(z·f(k)·2^k)` time, `z = Π(n_i+1)` —
  exponential in `k`, hence the heuristics that follow (fsa_part3, pp. 18–19).
- [[star-alignment]] builds an MSA from pairwise alignments to a center sequence and is a
  `(2 − 2/k)`-approximation of the optimal SP alignment for *good* cost functions
  (fsa_part3, pp. 20–22).
- [[progressive-alignment]] replaces the star by a **guide tree**; fast and widely used, but
  "once a gap always a gap" makes early errors permanent (fsa_part3, p. 23).
- A [[phylogenetic-tree]] distance is **additive** iff it satisfies the four-point condition,
  testable in `O(n⁴)` (fsa_part3, pp. 25–26).
- A distance is [[ultrametric]] (decidable in `O(n³)`) iff, for additive `d`, every leaf has the
  same distance from the root — the molecular clock hypothesis (fsa_part3, p. 29).
- [[upgma]] clusters by average linkage into a rooted binary tree in `O(n²)`; [[neighbor-joining]]
  uses `N_ij = d(i,j) − (r_i + r_j)` and builds an unrooted tree when rates vary
  (fsa_part3, pp. 27–30).

## Lecture content

### PSSMs (pp. 1–2)

Membership in a sequence family allows inference about function. The signal common to the family
is captured statistically from a multiple alignment of the members: column-wise relative
frequencies give an `|A| × m` matrix `M`. A word `w` of length `m` is scored by summing per-position
entries. The **PSSM searching problem** asks for all `j`, `1 ≤ j ≤ n−m+1`, with
`score(S[j..j+m−1], M) ≥ th`. Naive sliding window: `O(mn)`. Look-ahead scoring precomputes the
maximum achievable suffix score `σ_d` and aborts a window as soon as
`pfxscore_d(w,M) < th_d = th − σ_d`. Detail on [[position-specific-scoring-matrix]] and
[[pssm-lookahead-search]].

### Addenda to parts 1–2 (pp. 3–5)

- `lcslen(u,v) = (m + n − edist_δ(u,v)) / 2` — see [[longest-common-subsequence]].
- The **swap/transposition** operation `ab → ba` and the extra edit-graph edge
  `(i−2, j−2) → (i,j)` — see [[edit-operation]].
- The `Ψ_lr` / `Ψ_rl` partition example for `u = agtgcacacatc`, `v = atcacacttagc` — see
  [[maximal-matches-distance]].
- The q-gram code variant `code(w) = Σ_i wc(w[q−i+1])·r^{q−i}` with `wc(b) = 3 − b` (Watson–Crick
  complement) and the rolling update — see [[q-gram]].
- **Multi-eops lists** `[X_1 Y_1 … X_k Y_k]`, `X_i ∈ {R, D, I}`, `Y_i > 0` — a run-length encoding
  of an alignment, equivalently two equal-length lists. With `u`, `v` and the lists one reconstructs
  the alignment. See [[hirschberg-linear-space-alignment]].
- Three examples showing `qgdist > edist`, `qgdist < edist`, `qgdist = edist` — see
  [[q-gram-distance]].

### Significance of local alignments (pp. 6–9)

The question is whether an observed local alignment is due to chance. Mismatches are counted by
the [[hamming-distance]] `H(x,y)`, which requires equal lengths. A **k-LMA** `(ℓ, i, j)` is a pair
of length-`ℓ` substrings with `H ≤ k < ℓ`; it is extended to be left- and right-maximal. The
E-value counts the k'-LMAs with `ℓ' ≥ ℓ`, `k' ≤ k` expected between random sequences of the same
lengths under the uniform Bernoulli model (`p = 1/r`). For `k = 0` the count is `m·n·p^ℓ(1−p)`; in
general `m·n·C(ℓ,k)·p^{ℓ−k}·(1−p)^{k+2}`. Full derivation on [[alignment-significance]].

### Linear-space alignment (pp. 10–14)

`E^j_col` is one column of `E_δ`; only two columns are kept, giving the distance in `O(m)` space
(`firstEDtabcolumn` / `nextEDtabcolumn` / `evaluateEDtabcolumn`). Traceback is impossible without
the matrix, so divide and conquer is applied on the middle column `j_0 = ⌊n/2⌋`: the **crosspoint**
`(i_0, j_0)` on an optimal path is found via the row-index table `R_{j_0}`, computed column-wise
alongside `E^j_col`, with `i_0 = R^n_{j_0}(m)`. The crosspoints are collected in the
**crosspoint table** `C` of size `n`, `C(0) = 0`, `C(n) = m`, which encodes an optimal alignment.
Recursion depth `≤ log₂ n`, `cps(k) = 2^k − 1` crosspoints at level `k`. Total: `O(mn)` time,
`O(m + n + log n) = O(m+n)` space. Detail on [[hirschberg-linear-space-alignment]].

### Multiple sequence alignment (pp. 15–24)

An MSA of `S = {s_1, …, s_k}` is a tuple of `k` equal-length gapped strings that project back to
the inputs and have no all-gap column; `max n_i ≤ l ≤ Σ n_i`. Scoring is by **consensus cost**
(`Σ_j (k − count(j, cons[j]))`, well defined because all consensi of an MSA have equal cost) or by
**sum of pairs** (`δ_SP`). The exact solution is a `k`-dimensional edit recurrence minimising over
the `2^k − 1` nonzero bit vectors, in `O(z·f(k)·2^k)` time. Approximations: **star alignment**
around a center `c` minimising `totalscore`, merged with "once a gap always a gap", which is a
`(2 − 2/k)`-approximation for good cost functions and also yields a pruning bound for the exact DP;
**progressive/iterative alignment** along a guide tree; and **divide and conquer alignment** cutting
each sequence near its midpoint. See [[multiple-sequence-alignment]], [[multidimensional-dp-msa]],
[[star-alignment]], [[progressive-alignment]].

### Phylogenetic analysis (pp. 25–30)

A phylogenetic tree `T = (V, E, λ)` is an acyclic connected graph with a bijective leaf labelling
by taxa; rooted or unrooted; binary when every internal node has degree 3. Edge lengths encode the
putative order of speciation. Three method families: distance methods (the focus), maximum
parsimony, maximum likelihood. A distance is **additive** w.r.t. `T_X` if it is the sum of edge
lengths along leaf paths; additivity ⟺ the four-point condition, decidable in `O(n⁴)`. **UPGMA**
clusters by average linkage into a rooted binary tree with node heights `½ d(C_i, C_j)` in `O(n²)`;
it is correct exactly under the molecular clock hypothesis, i.e. when `d` is **ultrametric**
(decidable in `O(n³)`). **Neighbor Joining** corrects for varying rates using
`N_ij = d(i,j) − (r_i + r_j)` and produces an unrooted tree. See [[phylogenetic-tree]],
[[ultrametric]], [[upgma]], [[neighbor-joining]].

## Exercises

No numbered exercise sheet in part 3. The lecture examples and their generalizable lessons:

| p. | Example | Method | Answer | Lesson |
|---|---|---|---|---|
| 1 | Family `GTAT, GGCG, TTCG, GTCA, GTTA` | column-wise relative frequencies | the 4×4 PSSM reproduced on [[position-specific-scoring-matrix]] | a family becomes a matrix; scoring becomes a sum |
| 4 | `u = agtgcacacatc`, `v = atcacacttagc` | greedy left-to-right / right-to-left partitioning | the four sets `Ψ_lr(u,v)`, `Ψ_rl(u,v)`, `Ψ_lr(v,u)`, `Ψ_rl(v,u)` | different partitions, same size |
| 5 | three string pairs | compare `qgdist` and `edist_δ` | `>`, `<`, `=` are all possible | neither distance dominates; `qgdist` only bounds after division by `2q` |
| 21 | merging `c'/s'_1` with `c''/s''_2` | "once a gap always a gap" | the combined 3-row MSA | compatibility is achievable but optimality is not |
| 27 | 5 taxa | UPGMA average linkage | heights `½d(1,2)`, `½d(4,5)`, `½d(3,7)`, `½d(6,8)` | each merge fixes one internal node height |

## Pages created / updated by this ingest

- created: [[position-specific-scoring-matrix]], [[pssm-lookahead-search]],
  [[alignment-significance]], [[hirschberg-linear-space-alignment]],
  [[multiple-sequence-alignment]], [[multidimensional-dp-msa]], [[star-alignment]],
  [[progressive-alignment]], [[phylogenetic-tree]], [[ultrametric]], [[upgma]],
  [[neighbor-joining]]
- updated: [[edit-operation]], [[longest-common-subsequence]], [[q-gram]],
  [[maximal-matches-distance]], [[q-gram-distance]]

## Open questions / unclear in source

> [!question]
> - **p. 1** — the handwritten PSSM row labels transcribe as `A, T, G, T`; the only reading
>   consistent with a DNA PSSM is `A, C, G, T`. Assumed throughout.
> - **p. 3** — the swap cost is given twice and inconsistently: first `δ'(ab → ba) = 0` with
>   `δ'(ab → cd) > 0` when `a ≠ d` or `b ≠ c`; then `δ'(ab → cd) = 0` if `a = d` and `b = c`,
>   `= ∞` otherwise. The second form makes the swap free and everything else impossible.
> - **p. 4** — the exponent placement in the rolling q-gram update (`r^0`, `r^{−1}`, `r^{q−1}`)
>   is illegible and not dimensionally consistent with the part-2 formula.
> - **p. 4** — `Ψ_lr(v,u)` contains `ag(g)c`, with a bracketed character; reproduced as written.
> - **p. 9** — the exponent `(1−p)^{k+2}` is stated without derivation; the `+2` presumably covers
>   left- and right-maximality.
> - **p. 14** — the chain `O(m+n+log n) = O(m+n) = O(min{m,n})` is literally false as an equality
>   of expressions; it states the achievable column-wise bound.
> - **p. 27** — step 3 of the UPGMA example reads "Cluster `{{1,2}, 3}`" but gives the height as
>   `½ d(3,7)` where node `7 = {4,5}`, and the final merge is `½ d(6,8)` with `6 = {1,2}`. Only the
>   reading "merge `{3}` with `{4,5}` into node 8" is self-consistent; that reading is used on
>   [[upgma]].
> - **p. 2** — `k` denotes the average number of evaluated PSSM positions, while everywhere else in
>   part 3 `k` is the number of sequences or the mismatch bound. Symbol clash.

## Sources

- fsa_part3, pp. 1–30 (`raw/foundations_sequence_analysis_part3.pdf`)
