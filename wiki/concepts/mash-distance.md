---
title: Mash Distance
type: concept
module: GSA
units: [2]
sources: [fsa_part2]
tags: [gsa, minhash, poisson, genome-distance, alignment-free, exam-relevant]
status: solid
updated: 2026-10-21
---

# Mash Distance

## Definition

The **Mash distance** `MD_{q,s}(u, v)` derives the mutation rate of `u` and `v` from the
MinHash-estimated Jaccard index `J_est(u, v)` (fsa_part2, p. 15):

```
MD_{q,s}(u, v) =
    1                                                 , if J_est(u, v) = 0
    -(1/q) · ln( 2·J_est(u,v) / (J_est(u,v) + 1) )    , otherwise
```

Symbols: `q` — q-gram length; `s` — sketch size; `J_est` — the Jaccard index estimated from the
bottom-`s` sketches (see [[minhash-sketch]]); `d` — the per-base mutation rate.

## Why it matters

Mash estimates genome-scale distance from **constant-size sketches** instead of alignments:
all-against-all comparison of `k` sequences costs `O(n(q + log s) + k^2 s)` where only one term
depends on sequence length (fsa_part2, p. 13). This makes large-scale clustering of sequence sets
and overlap detection in long reads feasible (fsa_part2, p. 16).

## Model — Poisson mutations

The **Poisson distribution** `P_λ` is a discrete probability distribution with parameter `λ`, the
average number of events per interval (fsa_part2, p. 13):

```
P_λ(k events in a defined interval) = e^{-λ} · λ^k / k!
```

It applies when: `k` counts occurrences, events occur **independently**, the event rate is
**constant**, and no two events occur in the same sub-interval (fsa_part2, p. 13). The application
here is the number of mutations on a strand of DNA per unit length.

Mutations occur at single nucleotides randomly and independently with constant rate `d` per base,
so per q-gram the expected number is (fsa_part2, p. 14):

```
λ = q · d
```

A q-gram **matches** exactly when it carries zero mutations:

```
P_λ(matching q-gram) = P_λ(0 mutations in q-gram) = e^{-λ} · λ^0 / 0! = e^{-q·d}
```

## Derivation — from `J` to `d`

Let `n = (|u| + |v|) / 2` be the average length of `u` and `v` (fsa_part2, p. 14). Then:

1. Expected number of q-gram matches: `γ = n · e^{-q d}`.
2. Ignoring duplicate q-grams, `|U ∩ V| ≈ γ` and `|U ∪ V| ≈ 2n - γ`, so
   `J(u, v) ≈ γ / (2n - γ)`.
3. Substituting `γ`: `J(u, v) = e^{-qd} / (2 - e^{-qd})`.
4. Solving for the exponential: `2·J(u,v) / (J(u,v) + 1) ≈ e^{-qd}`.
5. Taking logarithms:

```
d ≈ -(1/q) · ln( 2·J(u,v) / (J(u,v) + 1) )
```

Since `J_est(u, v) ≈ J(u, v)` (fsa_part2, p. 15), substituting `J_est` yields the Mash distance.

```mermaid
flowchart LR
  G["q-gram sets"] --> S["bottom-s sketches"]
  S --> JE["J_est"]
  JE --> P["Poisson inversion"]
  P --> MD["Mash distance = mutation rate d"]
  MD --> C["clustering / overlap detection"]
```

## Pseudocode

```text
ALGORITHM MashDistance(u, v, q, s, h)
  Su <- MashSketch(u, q, s, h)                 // O(|u|(q + log s))
  Sv <- MashSketch(v, q, s, h)                 // O(|v|(q + log s))
  J  <- |Su ∩ Sv| / |Su ∪ Sv|                  // O(s)
  if J = 0:
    return 1
  return -(1 / q) * ln( 2 * J / (J + 1) )
```

## Complexity

| Operation | Time | Space |
|---|---:|---:|
| Sketch of `u` | `O(\|u\|(q + log s))` | `O(s)` |
| `J_est` from two sketches | `O(s)` | `O(1)` |
| **`MD_{q,s}(u, v)`** | `O((\|u\| + \|v\|)(q + log s))` | `O(s)` |
| All-against-all over `k` sequences, `n = Σ\|t_i\|` | `O(n(q + log s) + k^2 s)` | `O(k·s)` |

(fsa_part2, pp. 13, 15)

## Worked example

`q = 10`, sketches give `J_est(u, v) = 0.25`.

1. `2·J / (J + 1) = 0.5 / 1.25 = 0.4`
2. `ln(0.4) = -0.9163`
3. `d = -(1/10)·(-0.9163) = 0.0916`

So about 9.2 mutations per 100 bases, i.e. roughly 91 % nucleotide identity — directly comparable
to an [[average-nucleotide-identity]] value.

Sanity checks on the formula:

| `J_est` | `2J/(J+1)` | `MD_{10,s}` |
|---:|---:|---:|
| 1 | 1 | 0 (identical) |
| 0.5 | 0.667 | 0.0405 |
| 0.25 | 0.4 | 0.0916 |
| 0 | — | 1 (by definition) |

## Pitfalls & exam traps

> [!warning]
> - `J_est = 0` is a **special case** defined to return 1, because `ln(0)` is undefined — the
>   formula does not degrade gracefully.
> - `J = 1` gives `2/(1+1) = 1`, `ln 1 = 0`, so `MD = 0`. Check this to verify a remembered formula.
> - The denominator is `J + 1`, not `J - 1` or `2 - J`; it comes from `|U ∪ V| ≈ 2n - γ`.
> - The Poisson parameter is `λ = q·d` **per q-gram**, while `d` is per **base**. Mixing them up
>   changes the result by a factor `q`.
> - Mash models **point mutations only** — it is explicitly "no model of complex evolutionary
>   processes" (fsa_part2, p. 16). Indels, rearrangements and horizontal transfer are outside it.
> - The derivation ignores duplicate q-grams (fsa_part2, p. 14); repetitive genomes violate this.
> - `MD` is a distance (0 = identical), `J` is a similarity (1 = identical).

## Related

- [[minhash-sketch]] — produces the sketches and `J_est` that Mash consumes.
- [[jaccard-index]] — the exact quantity `J_est` approximates.
- [[q-gram]] — the unit over which the Poisson mutation model is defined.
- [[average-nucleotide-identity]] — the gold standard against which Mash distance is validated.
- [[fragment-assembly]] — overlap identification in long reads is a stated application.
- [[alignment-significance]] — the alignment-based alternative route to judging sequence relatedness.
- [[fsa_part2]] — source page for this unit.

## Sources

- fsa_part2, pp. 13–16 (raw file `raw/foundations_seqzence_analysis_part2.pdf`)
