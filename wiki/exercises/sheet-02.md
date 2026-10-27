---
title: Exercise Sheet 2 (Ü2)
type: exercise
units: [2]
sources: [gik_1_2]
tags: [exercises, posterior, sigmoid, exam-relevant]
status: draft
updated: 2026-10-15
---

# Exercise Sheet 2 (Ü2)

Source: gik_1_2, p. 26. Only task 2.1 is worked out in the notes.

---

## 2.1 — The posterior as a sigmoid

**Problem.** Given two competing models, "exterior" (`ext`) and "interior" (`int`), with
per-position frequencies `f_x[i]^ext` and `f_x[i]^int`, rewrite the posterior
`P(ext | x)` into a numerically stable form.

**Method.** Apply [[prior-posterior-probability|Bayes' theorem]], then factor out the numerator
and convert to log space ([[log-space-computation]]).

**Solution.**

```
                  (∏(i=1..n) f_x[i]^ext) P(ext)
P(ext | x) = ───────────────────────────────────────────────────────
             (∏(i=1..n) f_x[i]^ext) P(ext) + (∏(i=1..n) f_x[i]^int) P(int)
```

Divide numerator and denominator by the numerator:

```
                              1
P(ext | x) = ──────────────────────────────────────
                   (∏ f_x[i]^int) P(int)
              1 + ───────────────────────
                   (∏ f_x[i]^ext) P(ext)

           = ( 1 + (∏ f_x^int) P(int) / ((∏ f_x^ext) P(ext)) )^(−1)
```

Now push everything through `exp`/`log`:

```
P(ext | x) = exp( −log( 1 + exp( log( (∏ f_x^int) P(int) ) − log( (∏ f_x^ext) P(ext) ) ) ) )
```

with the two log-terms being plain sums:

```
ŷ = log P(int) + Σ(i=1..n) log f_x^int
p̂ = log P(ext) + Σ(i=1..n) log f_x^ext
```

giving the final form:

```
P(ext | x) = exp( −log( 1 + exp( ŷ − p̂ ) ) )
```

**Generalisable lesson.** This is a **[[sigmoid-function|sigmoid]] of the log-likelihood
difference `ŷ − p̂`** — the same S-curve introduced on the very first page of unit 1. Two
lessons in one:

1. A binary posterior is *always* a sigmoid in the log-odds. The quantity `p̂ − ŷ` is precisely
   a [[log-odds-score]] with priors included.
2. The form `exp(−log(1 + exp(·)))` never computes a product of `n` probabilities directly, so
   it does not underflow — the practical payoff of [[log-space-computation]].

---

> [!question]
> Only task 2.1 appears in these notes. If the official sheet 2 had further tasks (likely on
> [[viterbi-algorithm|Viterbi]] or [[posterior-decoding]] given the unit 2 content), they are
> missing here and should be added from the original sheet.

## Related

- [[prior-posterior-probability]] — the theorem applied.
- [[sigmoid-function]] — the form the answer takes.
- [[log-space-computation]] — the technique.
- [[log-odds-score]] — the quantity `p̂ − ŷ`.
- [[sheet-01]] — the previous sheet.

## Sources

- gik_1_2, p. 26.
