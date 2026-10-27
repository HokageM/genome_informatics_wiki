---
title: GIK 3-4
type: source
units: [3, 4]
sources: [gik_3_4]
tags: [rna, edit-distance, chaining, exercises]
status: solid
updated: 2026-10-15
---

# GIK 3-4

## Scope

Units 3-4 cover RNA secondary-structure prediction, minimum-free-energy folding, unit-cost edit distance, output-sensitive edit distance, OTU thresholding, and anchor-based sequence comparison with match chaining (gik_3_4, pp. 1-36).

## Key takeaways

- [[rna-secondary-structure]] is a non-crossing set of allowed base pairs with a minimum loop length (gik_3_4, p. 1).
- [[rna-secondary-structure-representation]] converts the same pair set between graph, mountain plot, Vienna dot-bracket, circle plot, and tree views (gik_3_4, pp. 2-3).
- [[nussinov-algorithm]] predicts one optimal structure under independent base-pair energy in `O(n^3)` time and `O(n^2)` space (gik_3_4, pp. 4-7).
- [[rna-free-energy-model]] and [[rna-loop-energy-dp]] refine this by charging hairpin, helical, bulge, and interior loops (gik_3_4, pp. 8-12).
- [[unit-edit-distance]] is the unit-cost insertion/deletion/substitution DP; neighbouring entries differ by at most one (gik_3_4, pp. 11-16).
- [[lazy-edit-distance]] computes only DP cells forced by the requested target cell (gik_3_4, pp. 14-15).
- [[output-sensitive-edit-distance]] uses fronts and `d`-paths to obtain `O(m+n+e^2)` time for distance `e` (gik_3_4, pp. 17-25).
- [[operational-taxonomic-unit]] clustering turns an identity threshold such as 97% into an edit-distance cutoff (gik_3_4, p. 25).
- [[anchor-based-sequence-comparison]] uses matches, [[match-chaining]], and gap alignment between anchors (gik_3_4, p. 26).
- [[match-chaining-algorithm]] gives quadratic and `O(b log b)` algorithms for global chaining; [[sheet-04]] extends this to local chaining with gaps (gik_3_4, pp. 29-36).

## Lecture content

### RNA secondary structures

An RNA sequence `w[1..n]` over `{A,C,G,U}` folds into a secondary structure `S_w`, a set of pairs `(i,j)`. The notes require `i<j`, each position used at most once, no crossing pairs, allowed base chemistry (`A-U`, `U-A`, `G-C`, `C-G`, `G-U`, `U-G`), and at least `l_min` enclosed bases (gik_3_4, p. 1). The non-crossing rule excludes pseudoknots and is what enables interval DP.

The notes list five representations: base-pair graph, mountain plot, Vienna notation, circle plot, and tree representation (gik_3_4, p. 2). Vienna notation is parsed with a stack: push each `(` and pair it with the next matching `)` (gik_3_4, p. 3).

### Minimum free energy

The first energy model assumes independent base pairs:

`E_fe(S_w)=Σ_(i,j in S_w) α(w[i],w[j])`,

where stabilising pairs have negative energy (gik_3_4, p. 4). The interval DP `E(i,j)` considers four cases: pair `(i,j)`, leave `i` unpaired, leave `j` unpaired, or split the interval; it is filled diagonal-wise and traced back to one optimal structure (gik_3_4, pp. 5-7).

The later recurrence adds loop context. A base pair can close a hairpin, continue a helix, or bound a bulge/interior loop; helical regions are stabilising while larger loops are penalised (gik_3_4, pp. 8-12). Considering arbitrary interior loops gives total runtime `O(n^4)`, while bounding loop size reduces practical work (gik_3_4, p. 9).

### Unit edit distance and output sensitivity

Unit edit distance uses match cost 0 and insertion, deletion, substitution cost 1. The notes first derive the standard DP and useful local properties, then use them for lazy evaluation near the main diagonal (gik_3_4, pp. 11-15).

The output-sensitive algorithm views alignments as edit-graph paths. A `d`-path has exactly `d` cost-1 edges. `front(h,d)` stores the farthest row on diagonal `h=j-i`, greedily extended over matching characters; the first `d` reaching `(m,n)` is the edit distance (gik_3_4, pp. 17-20). The notes also sketch two trimming strategies: regional alignment quality and front-head distance (gik_3_4, pp. 21-24).

### OTUs and match chaining

For metagenomics, an OTU threshold such as 97% identity is converted to `d_max=(1-θ/100)·(|z|+|u|)/2`; the edit-distance search can stop when this cutoff is exceeded (gik_3_4, p. 25).

Anchor-based comparison has three phases: compute matches, find a highest-scoring chain of collinear non-overlapping matches, and align the regions between anchors (gik_3_4, p. 26). The match-chaining problem is solved either as a longest path in a DAG, a simple quadratic DP, or an efficient dictionary sweep in `O(b log b)` time (gik_3_4, pp. 29-34).

## Exercises

- **Ü3.1** counts possible RNA secondary structures for length `n` under `l_min=1` and universal pairability; the notes give counts `1,1,2,4,8` for `n=1..5` (gik_3_4, p. 10). See [[sheet-03]].
- **Ü3.2/3.3** prove unit-edit DP neighbourhood properties and lazy-evaluation cases (gik_3_4, pp. 13, 16). See [[sheet-03]].
- **Ü3.4** traces `front(h,d)` values for `0≤d≤3` (gik_3_4, p. 27). See [[sheet-03]].
- **Ü4.1/4.2** define local chaining with gap penalties and give bounded-predecessor pseudocode (gik_3_4, pp. 35-36). See [[sheet-04]].

## Pages created / updated by this ingest

- created: [[gik_3_4]], [[rna-secondary-structure]], [[rna-secondary-structure-representation]], [[rna-free-energy-model]], [[nussinov-algorithm]], [[rna-loop-energy-dp]], [[unit-edit-distance]], [[lazy-edit-distance]], [[output-sensitive-edit-distance]], [[operational-taxonomic-unit]], [[anchor-based-sequence-comparison]], [[match-chaining]], [[match-chaining-algorithm]], [[sheet-03]], [[sheet-04]]
- updated: [[overview]], [[exam-prep]], [[index]], [[log]]

## Open questions / unclear in source

> [!question]
> Exact thermodynamic tables for loop-energy functions are not included; only the recurrence structure and named terms are present (gik_3_4, pp. 8-12).

> [!question]
> The output-sensitive trimming pages are schematic; the safe exact core is the untrimmed front algorithm (gik_3_4, pp. 21-24).

## Sources

- `raw/gik_3_4.pdf`, pages 1–36.
