---
title: GIK 6 Source Notes
type: source
units: [6]
sources: [gik_6]
tags: [source, suffix-array, fm-index, assembly, clustering]
status: solid
updated: 2026-10-15
---

# GIK 6 Source Notes

## Scope

Unit 6 completes the module. It extends unit-5 suffix-tree material to suffix arrays, LCP local
maxima, [[burrows-wheeler-transform|BWT]] and [[fm-index|FM-indexes]], then moves to shotgun
[[fragment-assembly]], overlap/string graphs and density-based clustering of gene-expression
profiles. The exercise-style material is filed as [[sheet-06]].

## Key takeaways

- [[supermaximal-repeat]] refines [[maximal-repeat-enumeration]] using LCP local maxima.
- [[suffix-array]] stores suffix order compactly and supports exact intervals.
- [[suffix-array-pattern-matching]] finds all occurrences as an `m`-interval in `SUF`.
- [[q-gram-suffix-array-construction]] buckets suffixes by fixed prefixes before sorting.
- [[burrows-wheeler-transform]] and LF mapping make suffix order reversible.
- [[fm-index]] compresses a full-text index; [[fm-index-backward-search]] searches right-to-left.
- [[fragment-assembly]] turns overlaps into layout and consensus, with repeats and errors as
  central obstacles.
- [[overlap-graph]], [[shortest-common-superstring]], [[greedy-fragment-assembly]] and
  [[string-graph]] are the graph-theoretic assembly chain.
- [[density-peak-clustering]] clusters expression profiles by local density and separation.

## Lecture content

### Suffix-array repeat analysis

Pages 1-3 revisit repeats from unit 5. A local maximum in the LCP table represents a maximal
repeat when its surrounding suffixes are pairwise distinct in the required context; filtering
those maximal repeats gives [[supermaximal-repeat]] (gik_6, pp. 1-2). For two strings `S` and
`S'`, MUMs can be computed on `T=S#S'$ using LCP intervals and side tests, connecting directly to
[[maximal-unique-match]] (gik_6, p. 3).

### Exact matching and suffix-array construction

Pages 4-6 present exact pattern matching as two binary searches for the suffix-array interval.
Page 7 gives a q-gram construction idea: count fixed-length prefixes, place suffixes into buckets,
then sort each bucket. Page 8 lists which applications can be solved by local maxima, bottom-up
suffix-tree traversal, or top-down matching, and notes compressed LCP storage (gik_6, pp. 4-8).

### BWT and FM-index

Pages 9-15 define the BWT from `SUF`, explain rank/LF mapping and reconstructing the string, then
use the BWT for exact pattern matching. The FM-index stores rank support plus sampled suffix-array
values; `get_rows` returns the interval, and `get_position` locates a sampled coordinate by LF
steps (gik_6, pp. 9-15).

### Fragment assembly

Pages 16-28 describe shotgun sequencing, the overlap-layout-consensus pipeline, assembly errors,
repeat ambiguity, unknown orientation, coverage, SCS, overlap graphs, greedy Hamiltonian-path
selection and string-graph reduction. The source explicitly says greedy assembly can fail and
shows a length-9 greedy superstring versus a length-8 optimum (gik_6, p. 26).

### Gene-expression clustering

Pages 29-32 switch to clustering expression profiles. Euclidean distance, cutoff density,
Gaussian-kernel density, nearest-higher-density distance `delta`, density peaks and runtime terms
are given. This is the final module topic and reconnects to earlier clustering-style ideas such
as [[operational-taxonomic-unit]] thresholds (gik_6, pp. 29-32).

## Exercises

The handwritten source does not mark a separate printed exercise sheet, but it contains
exercise-style algorithms and examples. They are consolidated as [[sheet-06]]:

- suffix-array intervals and BWT/LF examples;
- supermaximal/MUM tests from LCP local maxima;
- overlap-graph/SCS/greedy-assembly procedures;
- density-peak clustering formulas and runtimes.

## Pages created / updated by this ingest

- created: [[gik_6]], [[suffix-array]], [[suffix-array-pattern-matching]],
  [[q-gram-suffix-array-construction]], [[supermaximal-repeat]],
  [[burrows-wheeler-transform]], [[fm-index]], [[fm-index-backward-search]],
  [[fragment-assembly]], [[overlap-graph]], [[shortest-common-superstring]],
  [[greedy-fragment-assembly]], [[string-graph]], [[density-peak-clustering]], [[sheet-06]]
- updated: [[overview]], [[exam-prep]], [[index]], [[enhanced-suffix-array]],
  [[maximal-repeat-enumeration]], [[all-pairs-suffix-prefix-matching]], [[log]]

## Open questions / unclear in source

> [!question]
> The source does not contain an explicit sheet number or printed exercise headings for unit 6;
> [[sheet-06]] is therefore inferred from the unit's worked algorithmic material.

> [!question]
> Some dense LCP formulas on p. 3 are hard to disambiguate visually. The wiki records the robust
> interval tests but avoids over-specifying unclear index inequalities.

## Sources

- `raw/gik_6.pdf`, pages 1–32.
