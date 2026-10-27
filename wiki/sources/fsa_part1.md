---
title: "FSA Part 1 — Foundations of Sequence Analysis"
type: source
module: GSA
units: [1]
sources: [fsa_part1]
tags: [gsa, alignment, edit-distance, string-matching, assembly]
status: solid
updated: 2026-10-21
---

# FSA Part 1 — Foundations of Sequence Analysis

Raw file: `raw/foundations_sequence_analysis_part1.pdf` — handwritten lecture script, 29 pages.
Module: **GSA — Grundlagen der Sequenzanalyse** (Foundations of Sequence Analysis). This is a
**different module** from GIK (Genome Informatics); pages belonging to it carry `module: GSA`.

## Scope

Part 1 covers the pairwise-comparison core of the course (fsa_part1, pp. 1–29):

- problem catalogue and O-notation (p. 1);
- distances: Hamming, the edit distance model, alignments and their combinatorics (pp. 2–4);
- cost functions, edit distance, metric properties, Levenshtein distance (pp. 5–6);
- graphs, shortest paths in DAGs, the edit graph and the `E_δ` DP (pp. 7–12);
- score functions, local similarity, Smith–Waterman (pp. 13–16);
- approximate string matching, exact string matching with the bad-character rule (pp. 17–19);
- shotgun sequencing, read assembly, overlaps and the `Ov_δ` DP (pp. 20–24);
- general and affine gap costs, the layered (Gotoh) edit graph (pp. 25–29).

No exercise sheet is part of this source — the script is lecture notes only.

## Key takeaways

- **Hamming distance** `f_H(u,v)` counts mismatching positions and is only defined for equal
  lengths (p. 2) — [[hamming-distance]].
- **Edit operations** are pairs `(α,β) ∈ (A¹ ∪ {ε})² \ {(ε,ε)}`: deletions, insertions
  (together: indels) and replacements (p. 2) — [[edit-operation]].
- An **alignment** is a sequence of edit operations whose first/second components concatenate to
  `u`/`v`; its length obeys `m + n >= h >= max{m,n}` (pp. 2–3) — [[alignment]].
- The **number of alignments** satisfies a three-term recurrence and grows like the
  Stanton–Cowan numbers `(1+√2)^(2n+1)·√n`, far faster than the subsequence count
  `Subseq(m,n)` (pp. 3–4) — [[counting-alignments]].
- A **cost function** `δ` with symmetry and the triangle inequality makes `edist_δ` a **metric**
  on `A*`; unit cost gives the **Levenshtein distance**, and `edist_δ(u,v) = edist_δ(u⁻¹,v⁻¹)`
  (pp. 5–6) — [[cost-function]].
- The **edit graph** `G(u,v)` is a DAG in which path ≙ alignment and shortest path ≙ optimal
  alignment, so the edit distance problem *is* a shortest-path problem (pp. 7–9) —
  [[edit-graph]].
- The **`E_δ` DP** solves it in `O(mn)` time, `O(m)` space without traceback, `O(mn + z)` time
  for all optimal alignments, `O(min{m,n})` space by divide and conquer (pp. 10–12) —
  [[edit-distance-dp]].
- **Score functions** are maximised, not minimised, which is what makes local comparison possible;
  BLOSUM62 is the protein example (p. 13) — [[score-function]].
- **Local similarity** `loc_σ(u,v)` maximises over all substring pairs; brute force is `O(m³n³)`
  (pp. 13–15) — [[local-similarity]].
- **Smith–Waterman** computes `α_σ` with an extra `0` in the max and tracebacks to the first `0`,
  `O(mn)` (pp. 15–16) — [[smith-waterman]].
- **APSMP** sets `D_δ(0,j) = 0` for all `j` (free start in `t`) and reports every `j` with
  `D_δ(m,j) <= k` (pp. 17–18) — [[approximate-string-matching]].
- **Exact matching** is `O(nm)` naively; the **bad-character table** stores the distance of the
  rightmost occurrence of each character from the pattern end (p. 19) —
  [[naive-exact-matching]].
- **Read assembly** = overlap, layout, consensus; overlaps have four configurations and are
  computed by the `Ov_δ` DP with both borders free (pp. 20–24) — [[overlap-alignment]],
  [[fragment-assembly]].
- **General gap costs** `g*` need `O(mn(m+n))`; **affine gap costs** recover `O(mn)` via a
  three-layer `R/D/I` edit graph (pp. 25–29) — [[gap-cost-model]],
  [[gotoh-affine-gap-alignment]].

## Lecture content

### Problems and distances (pp. 1–2)

The course catalogue: sequence comparison, sequence matching, regular expression matching,
multiple sequence matching, approximate sequence matching. O-notation is defined as
`0 <= f(n) <= c·g(n)` for all `n >= n_0`. `edist_δ(u,v)` is announced as computable in `O(mn)`
and characterised as a measure in which matches between substrings depend strongly on their
relative positions — hence not suitable where that dependence is unwanted.

### The edit distance model (pp. 2–6)

Edit operations, indels, alignments as two-row matrices. The counting identities
`|u| + |v| = id + 2r` and the length lemma. `Aligns(m,n)` and `Subseq(m,n)` with their
asymptotics; the script notes `Aligns >> Subseq` but that this "does not matter in reality".
Cost functions with the axioms (symmetry, zero property, triangle inequality), unit cost and
Hamming cost (indels `= ∞`). `edist_δ` as a minimum over alignments; optimal alignments need not
be unique. Metric transfer from `δ` to `edist_δ`; Levenshtein distance; the reversal lemma.

### Graphs and the DP (pp. 7–12)

Paths, weights, `Δ(s,x)`, the shortest-path problem; negative edges and the shift trick (motivated
by chemical reaction networks). Optimal substructure and the triangle inequality for `Δ`. Shortest
paths in a DAG in `O(|V| + |E|)`. The edit graph with its three edge kinds, edge weights `δ`, and
the path ≙ alignment correspondence. Columnwise and rowwise topological orders. The `E_δ`
recurrence, constant time per entry, traceback via minimising edges stored in a bit vector, and
the space/time trade-offs including the divide-and-conquer variant.

### Local similarity (pp. 13–16)

Why global comparison fails on sequences with small coding regions and shared protein domains.
Why a minimum-distance formulation is vacuous (`ε` is a substring of everything). Score functions
and BLOSUM62. `loc_σ`, optimal local alignments, the `O(m³n³)` brute force, the "substring =
suffix of a prefix" observation, the `α_σ` matrix, condition (3.3), the recurrence with the extra
`0` and the "positive islands" picture. The three-step Smith–Waterman algorithm and the `calcMO`
origin-matrix variant.

### Matching (pp. 17–19)

APSMP with threshold `k`, the `D_δ` table, the free top row, the match criterion
`D_δ(m,j) <= k`, the traceback length bound `m + γ` with `γ <= k / min δ(ε→b)` and the resulting
`O(m·(m+γ))` space (`O(m·(m+k))` for unit costs). Exact matching: the naive `O(nm)` algorithm with
its worst case, and the bad-character table with `compute(p,t)`.

### Assembly (pp. 20–24)

Shotgun sequencing of 30 000 to millions of bases into reads of unknown orientation and position.
The three assembly phases. Error rate `ε` and the `2mε` error budget for reads of length `m`.
The four overlap configurations, `len(A) = (|u|+|v|)/2`, the error-rate criterion
`δ(A) <= k = C·(|f|+|f'|)`, the `Ov_δ` table with both borders free, the four path types with
their start and end nodes, and the row/column minimisation that maximises `len(A)`. The page-24
summary unifies global, local, approximate and overlap alignment as the *same* `O(mn)` table with
different admissible start and end nodes.

### Gap costs (pp. 25–29)

Segment-level indel events motivate gaps. General gap costs `g*(|w|)` with jump edges and
`O(mn(m+n))`. Affine gap costs `q + Σ δ`, the three-layer node set `(i,j,X)`, the gap-start vs.
gap-extension edge table, start and final node triples, the `A_afn` recurrences with `∞` guards,
and `O(mn)` time and space with constant factor 3.

## Exercises

None — this source is a lecture script without exercise tasks.

## Pages created / updated by this ingest

- created: [[hamming-distance]], [[edit-operation]], [[alignment]], [[counting-alignments]],
  [[cost-function]], [[edit-graph]], [[score-function]], [[local-similarity]],
  [[gap-cost-model]], [[edit-distance-dp]], [[smith-waterman]],
  [[approximate-string-matching]], [[naive-exact-matching]], [[overlap-alignment]],
  [[gotoh-affine-gap-alignment]]
- updated: [[fragment-assembly]], [[unit-edit-distance]]

## Open questions / unclear in source

- **p. 22, index bounds.** The overlap criterion is written `Ov_δ(i,n) <= k, 0 <= i <= n` and
  `Ov_δ(m,j) <= k, 0 <= j <= m`. Since `Ov_δ` is `(m+1) × (n+1)`, the ranges must be
  `0 <= i <= m` and `0 <= j <= n`. Flagged on [[overlap-alignment]].
- **p. 23, illegible bound.** The derivation of the overlap length ends with `len(A) >= ` and one
  unreadable symbol. Flagged on [[overlap-alignment]].
- **p. 16, truncated pseudocode.** `calcMO` trails off with "⋮" after the first case; the
  remaining cases and the tie-breaking rule are not written out. Flagged on [[smith-waterman]].
- **pp. 26 vs. 28–29, notation.** The gap initiation constant is `q` on p. 26 and `g` on
  pp. 28–29. Treated as the same constant throughout.
- **p. 26, naming.** The `O(mn(m+n))` general-gap-cost DP is annotated "Needleman–Wunsch
  algorithm", a name usually attached to the plain `O(mn)` global alignment DP. Flagged on
  [[gap-cost-model]].
- **p. 29, base case.** `A_afn(0,0,Y) <= g` for `Y ∈ {D,I}` is stated as an inequality rather
  than an assignment. Flagged on [[gotoh-affine-gap-alignment]].
- **p. 19, worked example.** The bad-character example strings are partly illegible; the example
  on [[naive-exact-matching]] is a reconstruction consistent with the drawn shift.
- **p. 7, weight shifting.** The `−min w(e)` shift to remove negative edges does not preserve
  shortest paths when candidate paths have different lengths, which they do in `G(u,v)`. Noted as
  an inference on [[edit-graph]].

## Sources

- fsa_part1, pp. 1–29 (`raw/foundations_sequence_analysis_part1.pdf`)
