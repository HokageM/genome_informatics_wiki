---
title: Overlap Alignment
type: algorithm
module: GSA
units: [1]
sources: [fsa_part1]
tags: [gsa, assembly, alignment, dynamic-programming, exam-relevant]
status: solid
updated: 2026-10-21
---

# Overlap Alignment

## Definition

An [[alignment]] `A` is an **overlap** (Überlappung) of two reads `f` and `f'` if there are
substrings `u` of `f` and `v` of `f'` such that `A` aligns `u` and `v` and at least one of
(fsa_part1, p. 21):

1. `u` is a **suffix** of `f` and `v` is a **prefix** of `f'`;
2. `u` is a **prefix** of `f` and `v` is a **suffix** of `f'`;
3. `u = f` and `v` is a **substring** of `f'` (`f` contained in `f'`);
4. `u` is a **substring** of `f` and `v = f'` (`f'` contained in `f`).

The **overlap length** is

```
len(A) = ( |u| + |v| ) / 2
```

Two reads **overlap with an error rate** if `δ(A) <= k = C · (|f| + |f'|)` for a constant `C`.
The **overlap problem** for a pair `f, f'` is to decide whether they overlap and, if so, to
compute an overlap `A` with `δ(A) <= k` (fsa_part1, p. 21).

## Why it matters

It is the **overlap phase** of shotgun read assembly (fsa_part1, p. 20): the entire target
sequence is reconstructed by assembling reads in the correct orientation and order, and the first
step is determining suffix/prefix overlaps for all pairs of reads. Reads are not exact — with an
error rate `ε` per base (`0 <= ε <= 1`) one allows `2mε` errors for two reads of length `m`
(fsa_part1, p. 20), hence *approximate* overlaps. See [[fragment-assembly]].

```mermaid
graph LR
  S["shotgun sequencing"] --> R["reads"]
  R --> O["overlap phase: pairwise Ov_delta"]
  O --> L["layout phase: multiple alignment with gap symbols"]
  L --> C["consensus phase: per-column base call"]
  C --> T["target sequence"]
```

## Construction / Mechanism

Let `|f| = m`, `|f'| = n`. Compute an `(m+1) × (n+1)` table `Ov_δ` with (fsa_part1, p. 22):

```
Ov_δ(i,j) = min{ edist_δ(u,v) | u is a suffix of f[1..i] and v = f'[1..j] ,
                                or  u = f[1..i] and v is a suffix of f'[1..j] }
```

There is an overlap of `f` and `f'` iff

1. `Ov_δ(i, n) <= k` for some `0 <= i <= m`, **or**
2. `Ov_δ(m, j) <= k` for some `0 <= j <= n`.

> [!warning]
> The script writes these ranges as `0 <= i <= n` and `0 <= j <= m` (fsa_part1, p. 22) — the
> index bounds are swapped. `Ov_δ` is `(m+1) × (n+1)`, so the correct ranges are `0 <= i <= m`
> for the right column and `0 <= j <= n` for the bottom row. Treat the page as a slip of the pen.

Prefixes and suffixes may use **indels at no cost**, which is encoded by the free borders
(fsa_part1, p. 22):

```text
Ov_δ(i,j) = 0                                                 , i = 0 or j = 0
          = min { Ov_δ(i-1, j)   + δ(f[i]  -> ε),
                  Ov_δ(i,   j-1) + δ(ε     -> f'[j]),
                  Ov_δ(i-1, j-1) + δ(f[i]  -> f'[j]) }        , else
```

## The four types of paths

(fsa_part1, p. 22; grid axes: `f'` horizontal `0..n`, `f` vertical `0..m`.)

| # | start node | end node | fragment picture |
|---|---|---|---|
| (1) | `(i', 0)` | `(m, j)` | `f` left, `f'` right — suffix of `f` ↔ prefix of `f'` |
| (2) | `(0, j')` | `(i, n)` | `f'` left, `f` right — prefix of `f` ↔ suffix of `f'` |
| (3) | `(0, j')` | `(m, j)` | `f` contained in `f'` |
| (4) | `(i', 0)` | `(i, n)` | `f'` contained in `f` |

So: the path starts anywhere on the **top row or left column** and ends anywhere on the **bottom
row or right column**; the corner-to-corner-excluded nodes marked `X` on p. 24 are not admissible.

## Determining `len(A)`

(fsa_part1, p. 23.) For all overlaps starting at `(i',0)` one **tracks the row** the path starts
from and **minimises the row number** whenever there is more than one minimising edge into the
current node. Symmetrically, for overlaps starting at `(0,j')` one minimises the **column
number**.

> **Minimising choice ⟶ maximising the overlap length.**

A smaller start row `i'` means more of `f` participates in the overlap, hence a larger
`len(A) = (|u| + |v|)/2`. The page ends with a bound `len(A) >= …` whose final symbol is
illegible.

> [!question]
> The concluding inequality on p. 23 reads `len(A) >= ` followed by a single unreadable
> handwritten symbol. The intended bound is not recoverable from the transcript.

## Pseudocode

```text
ALGORITHM OverlapAlignment(f[1..m], f'[1..n], δ, k)
  for i = 0 to m:  Ov[i][0] <- 0 ;  start[i][0] <- (i, 0)
  for j = 0 to n:  Ov[0][j] <- 0 ;  start[0][j] <- (0, j)
  for i = 1 to m:
    for j = 1 to n:
      Ov[i][j] <- min( Ov[i-1][j]   + δ(f[i] -> ε),
                       Ov[i][j-1]   + δ(ε    -> f'[j]),
                       Ov[i-1][j-1] + δ(f[i] -> f'[j]) )
      // among all minimising predecessors, inherit the one with the
      // smallest start row, breaking ties by smallest start column
      start[i][j] <- argmin over minimising predecessors (i'',j'') of start[i''][j'']
  best <- ∞
  for i = 0 to m:                      // right column : path types (2), (4)
    if Ov[i][n] < best:  best <- Ov[i][n] ;  (ei, ej) <- (i, n)
  for j = 0 to n:                      // bottom row : path types (1), (3)
    if Ov[m][j] < best:  best <- Ov[m][j] ;  (ei, ej) <- (m, j)
  if best > k:  return "no overlap"
  A <- Traceback(Ov, (ei, ej), stopping at row 0 or column 0)
  return (A, best, len(A))
```

## Invariant / correctness argument

Setting `Ov_δ(i,0) = Ov_δ(0,j) = 0` makes every node of row 0 and column 0 a free source, so
`Ov_δ(i,j)` is the minimum path weight from that source set to `(i,j)`. Paths from `(i',0)` are
labelled by alignments of `f[i'+1..i]` with `f'[1..j]` and paths from `(0,j')` by alignments of
`f[1..i]` with `f'[j'+1..j]` (fsa_part1, p. 9) — which is exactly the two-case definition of
`Ov_δ(i,j)`. Admitting as end nodes the whole bottom row and right column admits exactly the four
overlap configurations, by the optimal-substructure theorem (fsa_part1, p. 7).

## Variants

Within the unified picture of p. 24, the four pairwise-comparison tables differ **only** in which
nodes a path may start and end at:

| Table | Start nodes | End nodes | Objective |
|---|---|---|---|
| `E_δ` global ([[edit-distance-dp]]) | `(0,0)` | `(m,n)` | min cost |
| `α_σ` local ([[smith-waterman]]) | any | any | **max score** |
| `D_δ` approximate ([[approximate-string-matching]]) | whole top row | whole bottom row | min cost |
| `Ov_δ` overlap (this page) | top row ∪ left column | bottom row ∪ right column | min cost |

All compute an `(m+1) × (n+1)` table in `O(mn)` time (fsa_part1, p. 24).

## Complexity

| Operation | Time | Space |
|---|---:|---:|
| Fill `Ov_δ` | `O(mn)` | `O(mn)` |
| Decide overlap (scan bottom row + right column) | `O(m + n)` after the fill | `O(1)` |
| Traceback one overlap | `O(|A|)` | table required |
| All pairs of `N` reads of length `L` | `O(N² L²)` | `O(L²)` per pair |

> [!note] Inference
> The all-pairs line is the obvious consequence of applying the pairwise `O(mn)` DP to all
> `C(N,2)` read pairs; the script states only the per-pair cost (fsa_part1, p. 24).

## Worked example

`f = TTACG` (`m = 5`), `f' = ACGTT` (`n = 5`), **unit cost**, `k = 1`.

`Ov_δ`, rows `i = 0..5` (`f = T T A C G`), columns `j = 0..5` (`f' = A C G T T`):

| `i \ j` | **0** | **1** `A` | **2** `C` | **3** `G` | **4** `T` | **5** `T` |
|---|---:|---:|---:|---:|---:|---:|
| **0** | 0 | 0 | 0 | 0 | 0 | 0 |
| **1** `T` | 0 | 1 | 1 | 1 | 0 | 0 |
| **2** `T` | 0 | 1 | 2 | 2 | 1 | 0 |
| **3** `A` | 0 | **0** | 1 | 2 | 2 | 1 |
| **4** `C` | 0 | 1 | **0** | 1 | 2 | 2 |
| **5** `G` | 0 | 1 | 1 | **0** | 1 | 2 |

Sample cell: `Ov(4,2)`, `f[4] = C`, `f'[2] = C` (match):
`min( Ov(3,2)+1 = 2 , Ov(4,1)+1 = 2 , Ov(3,1)+0 = 0 ) = 0`.

Bottom row: the minimum is `Ov(5,3) = 0 <= k`. Traceback `(5,3) ← (4,2) ← (3,1) ← (2,0)`, so the
path starts at `(i',0) = (2,0)` and ends at `(m,j) = (5,3)` — **path type (1)**: a suffix of `f`
against a prefix of `f'`.

```
u  = f[3..5]  = ACG
v  = f'[1..3] = ACG
len(A) = (3 + 3) / 2 = 3
δ(A)   = 0
```

Layout: `TTACG` + `ACGTT` glued on the shared `ACG` gives `TTACGTT`.

## Pitfalls & exam traps

> [!warning]
> - **Both** row 0 and column 0 are zero here — that is the single difference from
>   [[approximate-string-matching]], where only row 0 is free.
> - The answer is read off the **bottom row and the right column**, never from `Ov_δ(m,n)` alone.
> - `len(A) = (|u| + |v|)/2` is an **average** of the two overlapping stretches; they can differ
>   in length when the overlap contains indels, so `len(A)` need not be an integer.
> - Tie-breaking matters: to maximise the overlap you must *minimise* the start row (resp.
>   column) among minimising edges (fsa_part1, p. 23).
> - The error budget `k = C·(|f| + |f'|)` scales with read length; a fixed `k` is wrong for
>   heterogeneous reads.
> - The orientation of a read is unknown (either strand), so in practice `f'` and its reverse
>   complement must both be tested (fsa_part1, p. 20).

## Related

- [[fragment-assembly]] — the overlap/layout/consensus pipeline this feeds.
- [[edit-distance-dp]] — same recurrence, fixed endpoints.
- [[approximate-string-matching]] — one free border instead of two.
- [[smith-waterman]] — fully free endpoints, maximising.
- [[edit-graph]] — the free borders are extra source/target nodes.
- [[cost-function]] — supplies `δ` and the threshold semantics `δ(A) <= k`.

- [[fsa_part1]] ? source page for this part of the module.

## Sources

- fsa_part1, pp. 20–24 (`raw/foundations_sequence_analysis_part1.pdf`)
