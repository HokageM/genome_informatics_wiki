---
title: gik_5
type: source
units: [5]
sources: [gik_5]
tags: [source, suffix-tree, suffix-array]
status: solid
updated: 2026-10-15
---

# gik_5

## Scope

Unit 5 covers suffix trees, their construction and representation, suffix-tree applications for
pattern matching and repeat/uniqueness problems, plus exercise sheet 5. It then introduces
enhanced suffix arrays as an array-based replacement for suffix trees (gik_5, pp. 1-45).

## Key takeaways

- [[suffix-tree]] compresses all suffixes of `S$` into `O(n)` space (gik_5, pp. 1-4).
- [[mccreight-suffix-tree-construction]] builds `ST(S$)` in `O(n)` time using suffix links
  (gik_5, pp. 10-15).
- [[suffix-tree-exact-matching]] verifies a pattern in `O(|P|)` and reports all leaves below the
  reached location (gik_5, p. 16).
- [[suffix-tree-approximate-matching]] explores edit operations on a suffix tree for small `k`
  (gik_5, pp. 17-19).
- [[minimal-unique-substring]] and [[maximal-unique-match]] turn branching/leaf conditions into
  primer and genome-anchor tasks (gik_5, pp. 20-24).
- [[all-pairs-suffix-prefix-matching]] solves all directed overlap queries in one generalised
  suffix tree (gik_5, pp. 25-32).
- [[maximal-repeat-enumeration]] reports all maximal repeats in `O(n+z)` time (gik_5, pp. 33-36).
- [[enhanced-suffix-array]] stores suffix-tree leaf order and LCP structure in arrays (gik_5,
  pp. 43-45).

## Lecture content

### Suffix-tree basics

The unit starts from the problem that a length-`n` string has `n^2` substrings. A trie over all
suffixes shares common prefixes; compacting non-branching paths gives a suffix tree. The sentinel
`$` is required so that suffixes become leaves and no suffix is nested as a prefix of another
(gik_5, pp. 1-4).

### Construction

The WOTD/simple top-down construction partitions remaining suffixes by their next character and
is cache-friendly and parallelisable, but can degrade to quadratic time. McCreight's construction
inserts suffixes in order and uses suffix links plus `ScanPrefix`/`LinkLoc` so all rescans over
the run are linear (gik_5, pp. 5-15).

### Search applications

Exact matching follows pattern characters from the root and enumerates descendant leaves.
Approximate matching recursively simulates insertion, deletion, match and mismatch while tracking
used edits, with a maximal-match table `mu` as a pruning device (gik_5, pp. 16-19).

### Uniqueness, overlaps and repeats

Minimal unique substrings cross from repeated branching nodes to leaf edges. MUMs in `S#S'` are
unique in both genomes and maximal in both directions, making them anchors before chaining. The
all-pairs suffix-prefix problem uses terminal edges and whole leaves to report overlaps, and
maximal repeat enumeration combines left-character leaf annotations at branching nodes (gik_5,
pp. 20-36).

### Representations and enhanced suffix arrays

Exercise notes describe pointer tables for explicit suffix trees. The lecture then introduces
`SUF` and `LCP` arrays; LCP intervals correspond one-to-one to internal suffix-tree nodes, so many
queries can be transferred from trees to arrays (gik_5, pp. 39-45).

## Exercises

### Sheet 5.1 — shortest absent words

Given alphabet `A` and string `S`, define absent words and compute `mu(A,S)`, the minimum length
of a word in `A+` not occurring in `S`. The solution performs a breadth-first search over
suffix-tree locations and returns when an alphabet character is missing (gik_5, p. 38).

### Sheet 5.2 — suffix-number intervals and representation

The exercise asks for storing the set of suffix numbers under every node in `O(n)` space by using
one interval `[a,b]` in the suffix array order per node. A DFS over leaves creates intervals and
merges child intervals bottom-up (gik_5, p. 41). Pages 39-42 also specify table fields for a
pointer representation of suffix trees.

## Pages created / updated by this ingest

- created: [[suffix-tree]], [[mccreight-suffix-tree-construction]],
  [[suffix-tree-exact-matching]], [[suffix-tree-approximate-matching]],
  [[minimal-unique-substring]], [[maximal-unique-match]],
  [[all-pairs-suffix-prefix-matching]], [[maximal-repeat-enumeration]],
  [[shortest-absent-word]], [[enhanced-suffix-array]], [[sheet-05]]
- updated: [[overview]], [[exam-prep]], [[index]], [[anchor-based-sequence-comparison]],
  [[match-chaining]], [[log]]

## Open questions / unclear in source

> [!question]
> The notes sketch some exercise pseudocode densely (especially p. 38 and p. 41); the wiki records
> the inferred BFS/DFS procedures but flags them as exercise-level reconstructions rather than
> verbatim algorithms.

## Sources

- `raw/gik_5.pdf`, pages 1–45.
