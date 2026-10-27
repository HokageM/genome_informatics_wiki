---
title: Exercise Sheet 1 (Ü1)
type: exercise
units: [1, 2]
sources: [gik_1_2]
tags: [exercises, mle, viterbi, exam-relevant]
status: solid
updated: 2026-10-15
---

# Exercise Sheet 1 (Ü1)

Source: gik_1_2, pp. 24–25. Tasks 1.1 and 1.3 are worked out in the notes.

---

## 1.1 — Two coins, which was used?

**Problem.** Two coins with probabilities `p` and `p'` for heads (*zwei Münzen p und p' für
Kopf*). Toss `ℓ` times (*ℓ Würfe*), observe `k` heads (*k Köpfe*). Case (a): `p = 1/5`,
`p' = 2/5`. Which coin was more likely used?

**Method.** Compute the likelihood of the observation under each coin and compare — the
binomial model from [[maximum-likelihood-estimation]]:

```
C(ℓ,k) = ℓ! / (k!(ℓ−k)!)

g(p, ℓ, k)  = P(toss k heads out of ℓ | p)  = C(ℓ,k) (1/5)^k (4/5)^(ℓ−k)
g(p', ℓ, k) = P(toss k heads out of ℓ | p') = C(ℓ,k) (2/5)^k (3/5)^(ℓ−k)
```

**Solution.** Ask when `g(p, ℓ, k) ≤ g(p', ℓ, k)`. The binomial coefficient `C(ℓ,k)` is common
to both sides and cancels:

```
(1/5)^k (4/5)^(ℓ−k)  ≤  (2/5)^k (3/5)^(ℓ−k)
```

Take logarithms (both sides positive, `log` strictly increasing):

```
k log(1/5) + (ℓ−k) log(4/5)  ≤  k log(2/5) + (ℓ−k) log(3/5)
```

Collect the `k` terms and solve:

```
            log(3/5) − log(4/5)
k  ≤  ℓ · ───────────────────────────────────────────
          log(1/5) − log(4/5) − log(2/5) + log(3/5)
```

**Answer.** If `k` satisfies the inequality, coin `p'` is the more likely one; **else** coin
`p`.

> [!note] Inference
> Evaluating the constant: the numerator is `log(3/4) ≈ −0.2877`; the denominator is
> `log((1/5)(3/5)) − log((4/5)(2/5)) = log(3/25) − log(8/25) = log(3/8) ≈ −0.9808`. The ratio is
> ≈ `0.2933`. So `p'` is favoured roughly when `k ≤ 0.293 ℓ` — note this is the *threshold where
> the inequality flips*; for `k` above it, the coin with the higher head probability wins, as
> intuition demands.

**Generalisable lesson.** Comparing two models on the same data: cancel the common
combinatorial factor, take logs to turn products into sums, and solve for a **linear threshold
in the sufficient statistic** `k`. Exactly the structure of the [[log-odds-score]] in unit 2.

---

## 1.3 — Computing in log space

**Problem.** Rewrite the HMM joint probability so that it is computed with logarithms, and adapt
[[viterbi-algorithm|Viterbi]] accordingly.

**Method.** Use `x = e^(log x)` and `log(a·b) = log a + log b`, hence
`a·b = e^(log a + log b)`.

**Solution.** Starting from the joint probability of an [[hidden-markov-model|HMM]] path:

```
P(x, π) = a_{0,π₁} · ∏(i=1..ℓ) e_{π_i}(x[i]) · a_{π_i,π_(i+1)}

        = e^( log( a_{0,π₁} · ∏(i=1..ℓ) e_{π_i}(x[i]) a_{π_i,π_(i+1)} ) )
        = e^( log(a_{0,π₁}) + Σ(i=1..ℓ) log( e_{π_i}(x[i]) a_{π_i,π_(i+1)} ) )
        = e^( log(a) + Σ(i=1..ℓ) [ log(e) + log(a) ] )
```

Viterbi in log space:

```text
log V(q,i) = log(e) + max{ log(V) + log(a) }
return e^P
```

**Generalisable lesson.** See [[log-space-computation]]. The transformation is valid for Viterbi
because `log` commutes with `max`; it does **not** carry over unchanged to
[[forward-algorithm|Forward]]/[[backward-algorithm|Backward]], which sum.

---

> [!question]
> Task 1.2 is not present in the notes — only 1.1 and 1.3 are worked out. Worth recovering from
> the official sheet if it covered something not already on [[maximum-likelihood-estimation]].

## Related

- [[maximum-likelihood-estimation]] — task 1.1.
- [[log-space-computation]], [[viterbi-algorithm]] — task 1.3.
- [[sheet-02]] — the next sheet.

## Sources

- gik_1_2, pp. 24–25.
