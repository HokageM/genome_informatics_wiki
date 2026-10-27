---
title: "FSA Fragenkatalog — Part 2 (§3–§5)"
type: exercise
module: GSA
units: [1, 2, 3]
sources: [fsa_fragenkatalog]
tags: [gsa, exam-relevant, exercise, gap-costs, alignment-free, significance]
status: solid
updated: 2026-10-21
---

# FSA Fragenkatalog — Part 2 (§3–§5)

Questions Q3.1–Q5.3 of the exam question catalogue with the handwritten answers
(fsa_fragenkatalog, pp. 4–6 for the questions, pp. 19–25 for the answers). Hub:
[[fsa-fragenkatalog]]. Previous part: [[fsa-fragenkatalog-1]]. Next: [[fsa-fragenkatalog-3]].

## §3 — Special gap costs (Spezielle Gapkosten)

Generalising linear gap costs, the price of that generality, and affine gap costs.

**Q3.1** How is the model of *linear gap costs* generalised to the *general gap-cost model*?
**A** (p. 19) Define, for `w ∈ A⁺`, a cost for a whole gap at once:

`δ*(w → ε) = δ*(ε → w) = f*(|w|)`

The linear model is the special case `f*(|w|) = |w| · δ(indel) = a · x`. All `D`s / all `I`s of a
gap are charged in one step ("alle D's / I's werden gemacht auf einmal"); the rest of the model
carries over unchanged. → [[gap-cost-model]]

**Q3.2** In which running time can the edit distance be determined under the general gap-cost
model?
**A** `O(mn(n+m))` (p. 19) — each cell must consider every gap length in its row and column.
→ [[gap-cost-model]]

**Q3.3** Define *affine gap costs*.
**A** (p. 19) For every gap, gap-**initiation** costs `g` are charged once, plus a per-symbol
extension cost. For `w ∈ A⁺`:

`δ_affine(w → ε) = g + Σ_{i=1}^{|w|} δ(w[i] → ε)`
`δ_affine(ε → w) = g + Σ_{i=1}^{|w|} δ(ε → w[i])`

→ [[gap-cost-model]], [[gotoh-affine-gap-alignment]]

> [!question]
> **p. 19 — second formula.** The handwriting writes the summand of the insertion case as
> `δ(ε → v[i])`, using `v` where `w` is the bound variable. Transcribed here as `δ(ε → w[i])`.

## §4 — Sequence comparison without alignments (Sequenzvergleich ohne Alignments)

Maximal matches, q-word profiles and distance, q-word coding, hit counting, Jaccard, sketches,
Poisson.

**Q4.1** Define the *decomposition of `v` with respect to `u`* (Zerlegung) and the maximal-matches
distance.
**A** (p. 20) A decomposition `Ψ` of `v` w.r.t. `u` is a sequence
`(w₁, c₁, …, w_r, c_r, w_{r+1})` of substrings `w₁,…,w_{r+1}` **of `u`** and characters
`c₁,…,c_r`, with `v = w₁ c₁ … w_r c_r w_{r+1}`. `|Ψ| = r`. `mmdist(v,u)` is the size `r` of the
minimal such decomposition. → [[maximal-matches-distance]]

**Q4.2.1** Give a pair of sequences showing that `mmdist` is not a metric. Justify.
**A** (p. 20) `v = cbaabdcb`, `u = dbcba`:

`Ψ_lr(v,u) = (cba, a, b, cb)` ⇒ `mmdist(v,u) = 2`
`Ψ_lr(u,v) = (ab, c, ba)` ⇒ `mmdist(u,v) = 1`

⇒ **not symmetric**, hence not a metric. → [[maximal-matches-distance]]

> [!question]
> **p. 20 — argument order.** The arguments `u`/`v` are overwritten in the handwriting; the point
> being made is only that the two directions yield different values.

**Q4.2.2** Define, as in the lecture, a metric based on the maximal-matches distance.
**A** `mmm(u,v) = log₂( (mmdist(u,v)+1) · (mmdist(v,u)+1) )` (p. 20).
→ [[maximal-matches-distance]]

**Q4.3** With which time and space can `mmdist(v,u)` be computed?
**A** Time `O(|A|(m+n))`, space `O(m)` (p. 20) — a walk over the suffix tree of `u`.
→ [[maximal-matches-distance]], [[suffix-tree]]

**Q4.4** In which relation does `mmdist` stand to the unit-cost edit distance, and how is this
used?
**A** (p. 21) `mmdist(v,u) ≤ edist_δ(v,u)` — a **lower bound** for `edist_δ`. It is used as a
**filter** where only unit edit distances below a threshold `k` matter: discard every pair with
`mmdist(v,u) > k` before running the expensive DP.
→ [[maximal-matches-distance]], [[unit-edit-distance]]

**Q4.5.1** Define the *q-word profile* of `u`.
**A** A map sending every `w ∈ A^q` to an integer, `P_{u,q}(w)` = number of distinct positions in
`u` at which `w` occurs (p. 21). → [[q-gram]]

**Q4.5.2** Define the *q-word distance* of `u` and `v` from the profiles.
**A** `qgdist(u,v) = Σ_{w ∈ A^q} | P_{u,q}(w) − P_{v,q}(w) |` (p. 21). → [[q-gram-distance]]

**Q4.6** For `q ≥ 2`, give two sequences showing that the q-word distance is not a metric.
**A** (p. 21) With `A = {a,c}` and `q = 2`:

| 2-word | `u = acaa` | `v = aaca` |
|---|---|---|
| `aa` | 1 | 1 |
| `ac` | 1 | 1 |
| `ca` | 1 | 1 |
| `cc` | 0 | 0 |

`qgdist(u,v) = 0` but `u ≠ v` — the zero property (identity of indiscernibles) fails.
→ [[q-gram-distance]]

**Q4.7** Describe precisely how q-words over an alphabet of size `r` are encoded by integers in
`[0, r^q − 1]`.
**A** (p. 22) With a lexicographically sorted alphabet:

1. Sort `A = {a₁, …, a_r}`, `a₁ < … < a_r`.
2. Encode characters: `code(a_ℓ) = ℓ − 1`.
3. Encode q-words: `code(w) = Σ_{i=1}^{q} code(w[i]) · r^{q-i}` for all `w ∈ A^q`.

→ [[q-gram]]

**Q4.8** Which properties of the encoding are exploited to compute the codes of all q-words of a
sequence efficiently?
**A** (p. 22) Successive q-words can be derived from each other in **constant time**:

`code(xc) = ( code(ax) − code(a)·r^{q-1} ) · r + code(c)`

i.e. drop the leading character's contribution, shift by one radix place, add the new character.
→ [[q-gram]]

**Q4.9** Time and space for `qgdist` of two sequences of lengths `m`, `n` over an alphabet of size
`r`?
**A** Time `O(m + n + r^q)`, space `O(m + n + r^q)` (p. 22). → [[q-gram-distance]]

**Q4.10** Describe an algorithm computing, in `O(m + n + r^q)` time and `O(r^q)` space, the total
number of pairs `(i,j)` with `1 ≤ i ≤ m-q+1`, `1 ≤ j ≤ n-q+1` and
`u[i..i+q-1] = v[j..j+q-1]`. Example: `q = 2`, `u = aaca`, `v = acacaacc` gives `6 = 1 + 3 + 2`.
**A** (p. 23) The answer sheet records the *naive profile* algorithm:

1. Build the q-gram profiles of `u` and `v` in two arrays `T_u`, `T_v` with
   `T_u[code(w)] = P_{u,q}(w)` and `T_v[code(w)] = P_{v,q}(w)` for all `w ∈ A^q`.
   Array initialisation `O(r^q)` time; computing the codes for `u` and `v` `O(m+n)` time;
   `O(r^q)` space.
2. Compute `z = Σ_{i=0}^{r^q − 1} | T_u[i] − T_v[i] |` in `O(r^q)` time, `O(1)` extra space.

⇒ `O(m + n + r^q)` time, `O(r^q)` space.

```text
ALGORITHM QWordArrays(u, v, q, r)
  for c = 0 to r^q - 1:  T_u[c] <- 0;  T_v[c] <- 0
  for i = 1 to m - q + 1:  T_u[code(u[i..i+q-1])] <- T_u[code(u[i..i+q-1])] + 1
  for j = 1 to n - q + 1:  T_v[code(v[j..j+q-1])] <- T_v[code(v[j..j+q-1])] + 1
  z <- 0
  for c = 0 to r^q - 1:  z <- z + |T_u[c] - T_v[c]|
  return z
```

> [!warning]
> The answer sheet solves the **q-word distance** (sum of absolute differences), not the question
> as posed. The hit count asked for is `Σ_c T_u[c] · T_v[c]`, which is exactly the
> `6 = 1·1 + 1·3 + 1·2` decomposition the question gives for `aaca` / `acacaacc`. The handwriting
> shows a `·` between `T_u[·]` and `T_v[·]` with a warning triangle drawn in the margin and a note
> that a minus sign is meant — so the sheet is internally undecided on this very point.

| 2-word | `T_u` (`aaca`) | `T_v` (`acacaacc`) | product |
|---|---|---|---|
| `aa` | 1 | 1 | 1 |
| `ac` | 1 | 3 | 3 |
| `ca` | 1 | 2 | 2 |
| `cc` | 0 | 1 | 0 |
| | | **total** | **6** |

→ [[q-gram]], [[q-gram-distance]], [[match-count]]

**Q4.11** In which relation does the q-word distance stand to the unit-cost edit distance, and how
is this used?
**A** (p. 23) `qgdist(u,v) / (2q) ≤ edist_δ(u,v)` — a lower bound, usable as a **filter**: when only
pairs with `edist_δ(u,v) ≤ k` are of interest, drop pairs whose `qgdist` is too large.
→ [[q-gram-distance]]

> [!question]
> **p. 23 — filter condition.** The sheet writes the discard rule as "drop pairs with
> `qgdist(u,v) ≥ k`". Rearranging the stated bound gives the correct form `qgdist(u,v) > 2qk`.
> Flagged as written.

**Q4.12** Define the Jaccard index `J(U,V)`; when is it undefined, minimal, maximal?
**A** (p. 24) `J(U,V) = |U ∩ V| / |U ∪ V|`.

- Undefined when `U = V = ∅` (then `J = 0/0`).
- **Minimal** `J = 0` iff `U ∩ V = ∅` (disjoint sets).
- **Maximal** `J = 1` iff `U = V`.

→ [[jaccard-index]]

**Q4.13** With which running time, in terms of sketch size `s`, `q ∈ O(log s)` and sequence length
`m`, can a `mash` sketch be computed? Justify.
**A** (p. 24) `O(m(q + log s)) = O(m log s)`, because:

- enumerate all q-grams of `u`: `O(m)` time, `O(q)` space;
- apply the hash function to all q-grams: `O(mq)`;
- keep the `s` smallest hash values in a min-heap: `O(m log s)`.

With `q ∈ O(log s)` the two dominant terms coincide, giving `O(m log s)`.
→ [[minhash-sketch]], [[mash-distance]]

**Q4.14** Define the *Poisson distribution* for a parameter `λ`. Which properties must events in an
interval have for the probability of `k` events to be Poisson-distributed?
**A** (p. 24) The sheet answers only the conditions, with `k` = number of occurrences of the event:

- events occur independently;
- the rate at which the event occurs is constant;
- two events never occur in the same sub-interval.

*The definition of the Poisson distribution itself is not given in the source.*
→ [[mash-distance]]

## §5 — Significance computation of alignments (Signifikanzberechnung von Alignments)

E-values as expected counts, the bijection for maximal exact hits, binomial counting.

**Q5.1** What is the E-value of a local alignment of `u` and `v` of length `ℓ` with `k`
differences? A general definition, no exact formula.
**A** (p. 25) The E-value is the **number of local alignments of length `ℓ' ≤ ℓ` with `k' ≤ k`
differences that one would expect between random sequences of the same lengths as `u` and `v`**:

`E-value = |{ (ℓ', i', j') | (ℓ', i', j') is a k'-LMA in u and v, ℓ' ≤ ℓ, k' ≤ k }|`

→ [[alignment-significance]], [[karlin-altschul-statistics]]

> [!question]
> **p. 25 — set-builder.** The expression is compressed in the handwriting; "LMA" is read as
> left-maximal / local maximal alignment. Reproduced as the best available reading.

**Q5.2** For E-values of exact matches, the set `M_{max ≥ ℓ}` of maximal hits of minimum length `ℓ`
was considered. Give the other set of equal size and sketch why the sizes agree.
**A** (p. 25) `|M_{max ≥ ℓ}| = |M_{ℓmax = ℓ}|`, the set of **left-maximal exact hits of length
exactly `ℓ`**. Proof idea: construct a bijection

`(q_h, i_h, j_h) ↦ (ℓ, i_h, j_h)` with `q ≥ ℓ`

A triple `(ℓ, i, j)` is left-maximal but not right-maximal precisely when `q > ℓ`; truncating each
maximal hit of length `q ≥ ℓ` to its first `ℓ` characters is injective and surjective onto the
left-maximal hits of length `ℓ`. → [[alignment-significance]]

**Q5.3** For a set `M` of size `n` and `k ≤ n`: how many `k`-element subsets does `M` have?
**A** (p. 25) `C(n,k) = n! / (k!(n−k)!)` — the binomial coefficient.
→ [[alignment-significance]]

> [!question]
> **p. 25 — variable name.** The handwriting writes `ℓ` where the question uses `n`, i.e.
> `binom(ℓ,k) = ℓ!/(k!(ℓ-k)!)`. Transcribed with the question's `n`.

## Related

- [[fsa-fragenkatalog]] — the hub: usage guidance, section map, complexity summary.
- [[fsa-fragenkatalog-1]] — previous part: the edit-distance model and its variants.
- [[fsa-fragenkatalog-3]] — next part: Fasta, Blast, PSSMs, linear-space alignment.
- [[gap-cost-model]] — the linear / general / affine hierarchy of §3.
- [[maximal-matches-distance]] — `mmdist`, `mmm` and the filter bound of Q4.1–Q4.4.
- [[q-gram-distance]] — profiles, coding and the `qgdist/(2q)` bound of Q4.5–Q4.11.
- [[mash-distance]] — the Jaccard/sketch/Poisson chain of Q4.12–Q4.14.
- [[alignment-significance]] — the E-value framework of §5.

## Sources

- `raw/foundations_sequence_analysis_fragenkatalog.pdf`, pp. 4–6 (questions), pp. 19–25 (answers)
