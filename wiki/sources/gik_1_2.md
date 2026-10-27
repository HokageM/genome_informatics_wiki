---
title: Source — gik_1_2.pdf (Units 1–2)
type: source
units: [1, 2]
sources: [gik_1_2]
tags: [source, probability, hmm]
status: solid
updated: 2026-10-15
---

# Source — `gik_1_2.pdf` (Units 1–2)

26 handwritten pages, English with occasional German. Lecture summary for units 1 and 2 plus
exercise sheets Ü1 and Ü2.

| Pages | Part |
|---|---|
| 1–11 | Unit 1 — probabilistic models, MLE, Bayes, EM |
| 12–23 | Unit 2 — Markov chains, HMMs and their algorithms |
| 24–25 | Exercise sheet Ü1 |
| 26 | Exercise sheet Ü2 |

## Scope

**Unit 1** builds the probabilistic toolkit: what a probabilistic model is, how to fit its
parameters from data when everything is observed (MLE), how to compare two models when you have
prior beliefs (Bayes), and how to fit parameters when something is hidden (EM).

**Unit 2** applies that toolkit to sequences. The motivating biological problem is CpG island
detection; the modelling answer is Markov chains, then HMMs; the algorithmic answer is Viterbi,
Forward, Backward, posterior decoding and Baum–Welch.

## Key takeaways

- A [[probabilistic-model]] assigns a probability to every member of a class of objects; its
  parameters `Θ` are derived by statistics.
- [[maximum-likelihood-estimation]] chooses `Θ` to maximise `P(D | Θ)`; in practice always work
  with the log-likelihood.
- [[prior-posterior-probability|Bayes' theorem]] converts likelihood + prior into a posterior.
  The casino-dice example shows a high likelihood can still yield a low posterior.
- [[expectation-maximization]] alternates expectation and maximisation to fit models with hidden
  variables; it converges only to a **local** maximum.
- A [[mixture-model]] with known sources is fitted by plain MLE; with hidden sources it needs EM.
- [[cpg-island|CpG islands]] motivate everything in unit 2: CpGs are rarer than independent
  probabilities predict.
- A [[markov-chain]] conditions each symbol on its predecessor; begin/end states remove the
  special treatment of the first and last symbol.
- The [[log-odds-score]] classifies a *given* segment by comparing the `+` and `−` chains;
  normalise by length.
- A [[hidden-markov-model]] merges the two chains into one model with hidden states, removing
  the need for a window size.
- The [[hmm-three-problems|three HMM problems]] are decoding ([[viterbi-algorithm|Viterbi]]),
  evaluation ([[forward-algorithm|Forward]]/[[backward-algorithm|Backward]]) and learning
  ([[baum-welch-algorithm|Baum–Welch]]).
- [[posterior-decoding]] is the alternative to Viterbi when many paths are comparably probable,
  and gives per-position island probabilities.
- [[hmm-parameter-training]] is plain counting when states are known and NP-hard when they are
  not; [[pseudocounts]] prevent zero probabilities in both cases.
- [[log-space-computation]] is the thread running through both units: it fixes underflow in
  Viterbi and reappears as a [[sigmoid-function|sigmoid]] in exercise 2.1.

## Lecture content

### Unit 1 (pp. 1–11)

- **p. 1** — Probabilistic model analysis: [[sigmoid-function|sigmoid and exponential
  functions]] with parameter effects. Probabilistic models: parameters from statistics,
  descriptions from probabilities. [[probability-basics|Events, sample space, the probability
  axioms]], inclusion–exclusion.
- **p. 2** — Model vs probabilistic model ([[probabilistic-model]]). `P(x) = ∏ q_x[i]`.
  [[probability-basics|Conditional and joint probability]], the dice example.
- **pp. 3–5** — [[maximum-likelihood-estimation]]: `Θ`, likelihood function, training data,
  `q_a = f_a/n`. Coin example (`ℓ = 20, k = 7 ⇒ p = 7/20`). Traffic-delay example: exponential
  PDF, log-likelihood, `λ = 1/t̄`.
- **p. 6** — [[prior-posterior-probability|Prior/posterior and Bayes' theorem]]; the casino
  loaded-vs-fair dice worked through to `P(D_ld | [6,6,6]) ≈ 0.21`.
- **pp. 7–10** — [[expectation-maximization]] and [[mixture-model|1-d mixture models]], with and
  without colouring; responsibilities, parameter updates, the log-likelihood stopping criterion.
- **p. 11** — Implementation: the E-step integral and the
  [[numerical-integration-midpoint|midpoint method]], with the `m = 1` shortcut that cancels ε.
  EM's generality.

### Unit 2 (pp. 12–23)

- **p. 12** — [[cpg-island|CpG islands]] and the two questions. [[markov-chain|Markov chains]]:
  definition, transition matrix, `P(x)`.
- **p. 13** — [[markov-chain|Begin and end states]]; determining `A⁺` and `A⁻` from labelled
  sequences.
- **p. 14** — [[log-odds-score]], the `β` table, length normalisation.
- **p. 15** — Why windows are bad; merging `+` and `−`; the [[hidden-markov-model]] quadruple;
  the dishonest-casino example.
- **p. 16** — Paths, joint probability `P(x, π)`, and the [[hmm-three-problems|three problems]].
- **p. 17** — [[viterbi-algorithm]] with full pseudocode and `O(|Q|²·ℓ)` complexity; the note to
  use logs.
- **p. 18** — [[forward-algorithm]].
- **p. 19** — [[backward-algorithm]] and the [[hmm-tables-comparison|three tables compared]].
- **p. 20** — [[posterior-decoding]]: `F·B/P(x)`, the alternative state sequence `π̂`, and
  `G(i|x)` for island detection.
- **pp. 21–22** — [[hmm-parameter-training]]: topology vs parameters, case 1 counting,
  overfitting, [[pseudocounts]]; case 2 is NP-hard; the model log-likelihood.
- **p. 23** — [[baum-welch-algorithm]] in full, with expected counts and local-maximum
  convergence.

## Exercises

See [[sheet-01]] (tasks 1.1, 1.3) and [[sheet-02]] (task 2.1).

- **1.1** — two coins `p = 1/5` vs `p' = 2/5`: cancel `C(ℓ,k)`, take logs, solve for a linear
  threshold in `k`. Lesson: model comparison reduces to a threshold on a sufficient statistic.
- **1.3** — rewrite `P(x, π)` with logarithms and adapt Viterbi. Lesson: `log` commutes with
  `max`.
- **2.1** — rewrite `P(ext | x)` as `exp(−log(1 + exp(ŷ − p̂)))`. Lesson: a binary posterior is
  a sigmoid in the log-odds.

## Pages created by this ingest

- concepts: [[probability-basics]], [[probabilistic-model]], [[sigmoid-function]],
  [[maximum-likelihood-estimation]], [[prior-posterior-probability]],
  [[numerical-integration-midpoint]], [[cpg-island]], [[log-odds-score]],
  [[hmm-three-problems]], [[hmm-tables-comparison]], [[posterior-decoding]],
  [[hmm-parameter-training]], [[pseudocounts]], [[log-space-computation]]
- models: [[mixture-model]], [[markov-chain]], [[hidden-markov-model]]
- algorithms: [[expectation-maximization]], [[viterbi-algorithm]], [[forward-algorithm]],
  [[backward-algorithm]], [[baum-welch-algorithm]]
- exercises: [[sheet-01]], [[sheet-02]]

Updated: [[index]], [[overview]], [[exam-prep]], [[log]].

## Open questions / unclear in source

> [!question]
> - **Task 1.2 is missing** from the notes (only 1.1 and 1.3 are worked out), and sheet 2 shows
>   only task 2.1. Worth recovering from the official sheets.
> - **Pseudocounts in Baum–Welch**: listed as algorithm inputs on p. 23 but absent from the
>   update formulas on the same page. Assumed to be added to `A_{k,q}` and `E_k(b)` before
>   normalisation — flagged as an inference on [[baum-welch-algorithm]].
> - **The loaded die's full distribution** is given as `P(6) = 1/2` on p. 6 and as
>   `p_i = 1/10, p_6 = 1/2` on p. 15. The two examples use slightly different loaded dice; p. 6
>   does not state the non-six probabilities explicitly.
> - **EM's "bad for many points"** (p. 7) is noted without a quantitative bound.

## Sources

- `raw/gik_1_2.pdf`, pages 1–26.
