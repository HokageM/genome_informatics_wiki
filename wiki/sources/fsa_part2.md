---
title: "FSA Part 2 — Foundations of Sequence Analysis"
type: source
module: GSA
units: [2]
sources: [fsa_part2]
tags: [gsa, source, alignment-free, database-search, exam-relevant]
status: solid
updated: 2026-10-21
---

# FSA Part 2 — Foundations of Sequence Analysis

Raw file: `raw/foundations_seqzence_analysis_part2.pdf` — **note the typo "seqzence" in the actual
filename**; the correct spelling would be "sequence". 26 handwritten pages, German with English
fragments, rendered into English for this wiki.

> [!warning]
> This is the **GSA** module (Grundlagen der Sequenzanalyse / Foundations of Sequence Analysis),
> *not* the GIK (Genome Informatics) module that covers units 1–6 elsewhere in this wiki. Pages from
> this source carry `module: GSA` and `units: [2]` (= FSA part 2).

## Scope

Part 2 covers three blocks:

| Pages | Block |
|---|---|
| 1 | Longest common subsequence and the LCS graph |
| 2–4 | Maximal matches: partitions, `mmdist`, the `mmm` metric, filtering for edit distance |
| 5–9 | q-gram comparison: profiles, `qgdist`, integer coding, match count, weighted substring matching |
| 10–16 | Mash / MinHash: Jaccard index, sketches, Poisson mutation model, Mash distance, ANI |
| 17–20 | FASTA database search: hot spots, `h_w` table, diagonal runs, DAG |
| 21–26 | BLAST: five stages, `Env_k(w)`, hit pairs, ungapped/gapped extension, E-value |

The unifying arc: exact `O(mn)` alignment is too slow for databases and for genome-scale
all-against-all comparison, so the unit builds **cheap surrogate measures** (`mmdist`, `qgdist`,
`J`, `MD`) that bound or approximate edit distance, and **filter cascades** (FASTA, BLAST) that
spend the expensive DP only on survivors.

## Key takeaways

- [[longest-common-subsequence]] — the match-maximising path in the LCS graph; diagonal edges exist
  **only** where `u[i] = v[j]`, weight `+1` (p. 1).
- [[maximal-matches-distance]] — minimum number of marked characters in a partition of `v` into
  substrings of `u`; `mmdist(v,u) <= edist_δ(v,u)`, computable in `O(|A|(m+n))` from a suffix tree
  (pp. 2–4).
- The `mmm` metric symmetrises the asymmetric `mmdist` as
  `log_2((mmdist(u,v)+1)·(mmdist(v,u)+1))` (p. 3).
- [[q-gram]] — the q-gram profile `P_{u,q}` and the bijective coding
  `code(w) = Σ code(w[i])·r^{q-i}` with a **constant-time** rolling-window update (pp. 5–6).
- [[q-gram-distance]] — `L1` distance between profiles; symmetry and triangle inequality hold, the
  zero property does not, so it is **not a metric**; `O(m+n+r^q)`; filter bound
  `qgdist/(2q) <= edist_δ` (pp. 5, 7).
- [[match-count]] — graded window agreement `MC(i,j)` with an `O(mn)` diagonal incremental
  recurrence replacing the naive `O(mnq)` (p. 8).
- [[weighted-substring-matching]] — all substrings of exact weight `w` in `O(|S|)` by a two-pointer
  sliding window (p. 9).
- [[jaccard-index]] — `J(U,V) = |U∩V|/|U∪V|` on q-gram **sets**, computed in `O(m+n)` by
  code + radix sort + merge; faster than `qgdist` (p. 10).
- [[minhash-sketch]] — bottom-`s` hash sketch; `J_est` in `O(s)`; MinHash does **not** improve
  running time over `qgdist`, it **saves space**; plain q-gram coding is biased toward poly-A
  (pp. 11–13).
- [[mash-distance]] — Poisson mutation model gives `P_λ(match) = e^{-qd}` and hence
  `d ≈ -(1/q)·ln(2J/(J+1))`; `MD_{q,s}(u,v) = 1` when `J_est = 0` (pp. 13–15).
- [[average-nucleotide-identity]] — the alignment-based gold standard validating Mash distance
  (p. 16).
- [[fasta-database-search]] — hot spots per diagonal, `score_fasta = max_d hotsp`, the two-scan
  `P(c)`/`H` bucket construction of `h_w(c)`, diagonal runs, and a DAG with negative edges whose
  maximal-weight path selects a substring pair for global alignment (pp. 17–20).
- [[blast]] — five stages, blast hits by **score** not identity, `Env_k(w)` enumerated by a pruned
  trie and stored as a bit vector `V` plus `Pos(s)`, diagonal hit pairs via `f(d)` and `δ`, ungapped
  extension with drop-off `X_d` yielding MSPs, affine-gapped extension, E-value (pp. 21–26).
- [[karlin-altschul-statistics]] — `S' = (λS − ln K)/ln 2`, `E = m·n / 2^{S'}`; `λ`, `K` from random
  simulation (p. 26).

## Lecture content

### Longest common subsequence (p. 1)

A common subsequence is a list of strictly increasing index pairs with matching characters. The LCS
graph is the `(m+1)×(n+1)` lattice with `I`/`D` edges of weight 0 and conditional diagonal match
edges of weight `+1`; the LCS length is the weight of the maximising path. The DP recurrence is
`LCS_δ(i,j) = LCS_δ(i-1,j-1)+1` on equality and `max{LCS_δ(i-1,j), LCS_δ(i,j-1)}` otherwise, with
zero boundaries, in `O(mn)` space. → [[longest-common-subsequence]]

### Maximal matches (pp. 2–4)

A partition of `v` with respect to `u` writes `v = w_1 c_1 … w_r c_r w_{r+1}` with every `w_h` a
substring of `u`; `mmdist(v,u)` is the size `r` of a minimal partition. The greedy left-to-right and
right-to-left partitions `Ψ_lr`, `Ψ_rl` are both minimal and are computed by walking the suffix tree
of `u` (resp. of `u^{-1}`) in `O(|A|(m+n))` time, `O(m)` space. `mmdist` is asymmetric and therefore
not a metric; `mmm` repairs this. Converting a partition into an alignment (submatches ↔ `R`, marked
characters ↔ `I`) proves `mmdist(v,u) <= edist_δ(v,u)`, which licenses its use as a **filter**: for
a threshold `k`, discard any pair with `mmdist > k` before running the DP.
→ [[maximal-matches-distance]]

### q-gram comparison (pp. 5–7)

A q-gram is a length-`q` substring; the profile counts occurrence positions for every `w ∈ A^q`.
`qgdist` sums `|P_{u,q}(w) − P_{v,q}(w)|` over `A^q`. The ordered alphabet gives a bijective
radix-`r` coding of q-grams into `[0, r^q−1]`, updated in `O(1)` per sliding-window step. The
distance algorithm accumulates both profiles into arrays, collects the occurring codes `C`, and sums
differences over `C` only: `O(m+n+r^q)` time and space, optimal when `r^q ∈ O(m+n)`. The bound
`qgdist/(2q) <= edist_δ` makes it a second filter. → [[q-gram]], [[q-gram-distance]]

### Match count (p. 8)

`MC(i,j)` counts matching characters between two aligned windows of length `q`. Naively `O(mnq)`;
computing the first row and column directly and then applying
`MC(i,j) = MC(i-1,j-1) − early_match + new_match` along each diagonal gives `O(mn)`.
→ [[match-count]]

### Weighted substring matching (p. 9)

With `σ(u) = Σ σ(u[i])`, find `Sol(σ,S,w)`, all `(start, length)` pairs with window weight exactly
`w`. A two-pointer window extends right, shrinks left while too heavy, and reports on equality — all
solutions in `O(|S|)`. → [[weighted-substring-matching]]

### Mash: Jaccard, sketches, Poisson (pp. 10–16)

`J(u,v) = J(G_q(u), G_q(v))` on q-gram **sets**; coding + radix sort + merge gives `O(m+n)`. Mash
replaces the full set by a **sketch**: hash every q-gram and keep the `s` smallest values, costing
`O(m(q + log s))` time and `O(s)` space; `J_est` is the Jaccard index of two sketches. The
order-preserving q-gram code is collision-free but biased (poly-A always smallest), so a
pseudo-random hash is required for the sketch to be a random sample. For `k` sequences,
all-against-all costs `O(n(q + log s) + k^2 s)` — only one term depends on length.

Mutations are modelled as Poisson events with rate `d` per base, hence `λ = qd` per q-gram and
`P(match) = e^{-qd}`. With `n` the average length and `γ = n e^{-qd}` the expected number of q-gram
matches, `J ≈ γ/(2n−γ) = e^{-qd}/(2−e^{-qd})`, inverted to
`d ≈ −(1/q)·ln(2J/(J+1))`. Substituting `J_est` defines `MD_{q,s}(u,v)`, with the special case
`MD = 1` when `J_est = 0`. Reliability is established against the gold standard `ANI(u,v)`, the
mean over significant local alignments of `1 − 2δ(A)/(|A.u|+|A.v|)`. Mash captures **no** complex
evolutionary processes; its applications are long-read overlap detection and large-scale clustering.
→ [[jaccard-index]], [[minhash-sketch]], [[mash-distance]], [[average-nucleotide-identity]]

### FASTA (pp. 17–20)

Goal: find database sequences with local similarities to a query `w`, running Smith–Waterman on only
a few. A good filter must be fast to evaluate, highly selective, and must not reject true
similarities. FASTA counts, per diagonal `d`, the exact q-gram matches (hot spots);
`score_fasta = max_d hotsp(u,w,d)`. The query is preprocessed into `h_w(c)`, the positions of each
q-gram code, built in `O(n + r^q)` by a two-scan bucket construction: first scan counts and forms
prefix sums `P(c)`, second scan writes positions at `H[P(c)]` and increments `P(c)`. Each database
sequence costs `O(r^q + m + n + Σ_d hotsp)`. Survivors (`score_fasta > k`) are refined into diagonal
runs (hot spots on a diagonal separated by small gaps, scored `+` hotspots `−` gaps), assembled into
a DAG with negative-weight edges under the corner condition `h_1(r) < l_1(r')` and
`h_2(r) < l_2(r')`; a maximal-weight path defines a substring pair that is globally aligned and
rejected if its score is `<= k`. Crucially, FASTA requires matching q-grams at the **same
displacement** in both sequences — the opposite of the order-free `qgdist` model.
→ [[fasta-database-search]]

### BLAST (pp. 21–26)

Five increasingly fine-grained stages. **Stage 1** finds blast hits — index pairs `(i,j)` with
`score_σ(u[i..i+q-1], w[j..j+q-1]) >= k`, i.e. scored rather than exact seeds, with no indels. The
query is preprocessed into the `k`-environment `Env_k(w)`, enumerated by a trie over `A^q` that
maintains a partial score at each node and prunes any subtrie that can no longer reach `k`; nodes at
depth `q` with score `>= k` are environment members. `Env_k(w)` is stored as a bit vector `V` of
`r^q` bits plus position lists `Pos(s)` (the analogue of FASTA's `h_w`). Preprocessing depends only
on `w` and is done once; each database sequence is then scanned in `O(|u| + z)`, `z` = number of
hits.

**Stage 2** pairs hits on the same diagonal `d = j − i` within maximal distance `δ`, maintaining
`f(d)` = position of the last hit on `d`. **Stage 3** extends from the midpoint of a hit pair both
ways, tracking `X_max` and stopping at `X_max − X_d`; the result is a maximum segment pair (MSP),
which proceeds if its significance beats a threshold. **Stage 4** performs a gapped extension from a
seed point inside the high-scoring ungapped region using affine gap scores, again stopping at the
drop-off. **Stage 5** collects traceback, displays alignments and reports Karlin–Altschul E-values.
→ [[blast]], [[karlin-altschul-statistics]]

## Exercises

No exercise sheet is part of this source — pp. 1–26 are lecture script only. The worked numeric
illustrations (the `aaca` q-gram profile on p. 5, the `P(c)`/`H` bucket trace on p. 19, the `Env_k`
trie on p. 22) are reproduced as `## Worked example` sections on [[q-gram]],
[[fasta-database-search]] and [[blast]] respectively.

## Pages created / updated by this ingest

- created: [[longest-common-subsequence]], [[maximal-matches-distance]], [[q-gram]],
  [[q-gram-distance]], [[match-count]], [[jaccard-index]], [[mash-distance]],
  [[average-nucleotide-identity]], [[karlin-altschul-statistics]],
  [[weighted-substring-matching]], [[minhash-sketch]], [[fasta-database-search]], [[blast]]
- updated: none (this ingest only adds pages)

## Open questions / unclear in source

> [!question]
> **p. 5 — q-gram count.** The page writes "`# q-grams = r^q − 1`". The number of q-grams over an
> alphabet of size `r` is `r^q`; `r^q − 1` is the largest **index** in the 0-based list. Treated as a
> transcription of the index range on [[q-gram]].

> [!question]
> **p. 7 — space of step 2.** The marginal note for the set `C` is illegible; best reading is
> `O(m+n)`. The stated total `O(m+n+r^q)` is consistent with that reading.

> [!question]
> **p. 8 — match-count indexing.** The definition uses windows `u[i+1..i+q]` but the recurrence's
> leaving/entering pairs (`u[i-1]`, `u[i+q-1]`) match windows `u[i..i+q-1]`. The page is off by one
> between the two formulations; flagged on [[match-count]].

> [!question]
> **p. 18 — loop variable roles.** The database-processing loop names `j` for positions in `u` and
> `i` for positions in `w`, while the `hotsp` definition on p. 17 uses `i` in `u` and `j` in `w`. The
> diagonal `j − i` therefore flips sign between the two pages; flagged on [[fasta-database-search]].

> [!question]
> **p. 19 — bucket trace row 3.** With `P(ac) = 5` the third scan step should write to slot 5, but
> the page records slot 3. Either the prefix-sum row or the slot column is in error; flagged on
> [[fasta-database-search]].

> [!question]
> **p. 22 — trie node scores.** The annotated partial scores (`a/2`, `aa/11`, `ac/4`) are not
> additively consistent with a single second-character substitution score. Reproduced as written and
> flagged on [[blast]].

> [!question]
> **p. 9 — positivity of `σ`.** The sliding-window correctness argument requires `σ(c) > 0` for all
> characters; the page does not state this precondition. Flagged on [[weighted-substring-matching]].

> [!question]
> **p. 12 — "`|S(U) ∪ S(V)|` in `O(1)`".** This holds only because both sketch sizes are known and
> the intersection has just been computed; it is not an independent constant-time set union.

## Sources

- `raw/foundations_seqzence_analysis_part2.pdf`, pp. 1–26 (cited throughout as `fsa_part2, p. N`)
