---
title: Exam Prep
type: meta
units: [1, 2, 3, 4, 5, 6]
sources: [gik_1_2, gik_3_4, gik_5, gik_6, fsa_part1, fsa_part2, fsa_part3, fsa_fragenkatalog]
tags: [exam-relevant]
status: draft
updated: 2026-10-21
---

# Exam Prep

Running cheat sheet, grown on every ingest. Each row links to the page carrying the full
treatment; this page stays short enough to read in one sitting.

Covers **two modules**. Everything down to the GIK "Exam traps" section is
**GIK — Genome Informatics**; the **GSA — Foundations of Sequence Analysis** cheat sheet starts
further down. For GSA, the single densest drilling resource is the question catalogue
[[fsa-fragenkatalog]].

## Definitions

| Term | One-line definition | Page |
|---|---|---|
| Probabilistic model | a model producing different outcomes with different probabilities | [[probabilistic-model]] |
| Likelihood | `L(Θ\|D) = P(D\|Θ)`, varied over `Θ` with `D` fixed | [[maximum-likelihood-estimation]] |
| MLE | the `Θ` maximising the probability that the model generated the data | [[maximum-likelihood-estimation]] |
| Prior / posterior | belief before / after seeing the data | [[prior-posterior-probability]] |
| Bayes' theorem | `P(X\|Y) = P(Y\|X)P(X)/P(Y)` | [[prior-posterior-probability]] |
| EM | iterative E/M scheme for parameter estimation with hidden variables | [[expectation-maximization]] |
| Responsibility | `r_i = P(r\|x_i)`, a soft component assignment | [[mixture-model]] |
| CpG island | region with far more CpG dinucleotides than expected | [[cpg-island]] |
| Markov chain (order 1) | pair `(S,A)` with `P(x_i = t \| x_{i−1} = s) = a_{s,t}` | [[markov-chain]] |
| HMM | quadruple `(Σ,Q,A,e)`; symbols observed, states hidden | [[hidden-markov-model]] |
| Path `π` | a sequence of states in the HMM | [[hidden-markov-model]] |
| Log-odds score | `log P(x\|+)/P(x\|−)`, normalised by `\|x\|` | [[log-odds-score]] |
| Pseudocount | artificial count added before normalising, avoiding zeros | [[pseudocounts]] |
| RNA secondary structure | non-crossing set of allowed RNA base pairs with a minimum loop length | [[rna-secondary-structure]] |
| Minimum free energy | choose the legal RNA structure with smallest free energy | [[rna-free-energy-model]] |
| Unit edit distance | minimum number of unit-cost insertions, deletions and substitutions | [[unit-edit-distance]] |
| `front(h,d)` | farthest row reached on diagonal `h=j-i` with `d` edits | [[output-sensitive-edit-distance]] |
| OTU | operational sequence cluster above an identity threshold | [[operational-taxonomic-unit]] |
| Match chain | collinear non-overlapping matches ordered in both sequences | [[match-chaining]] |
| Suffix tree | compact trie of all suffixes of `S$`, with one leaf per suffix | [[suffix-tree]] |
| Suffix link | edge from node `aw` to node `w` used to avoid rescanning in McCreight | [[mccreight-suffix-tree-construction]] |
| MUM | exact match unique in both genomes and left/right maximal | [[maximal-unique-match]] |
| MUS | substring occurring once whose proper prefixes are not unique | [[minimal-unique-substring]] |
| Suffix-prefix overlap | suffix of one sequence equal to prefix of another | [[all-pairs-suffix-prefix-matching]] |
| LCP array | adjacent-suffix longest common prefix table | [[enhanced-suffix-array]] |
| Suffix array | lexicographically sorted suffix-start positions | [[suffix-array]] |
| Supermaximal repeat | maximal repeat not contained in any other maximal repeat | [[supermaximal-repeat]] |
| BWT | preceding-character string of sorted suffixes | [[burrows-wheeler-transform]] |
| LF mapping | map from BWT row to corresponding first-column row with same occurrence rank | [[burrows-wheeler-transform]] |
| FM-index | compressed BWT-based full-text index with rank support and sampled suffix-array entries | [[fm-index]] |
| Fragment assembly | reconstruct a target sequence from unoriented shotgun fragments | [[fragment-assembly]] |
| SCS | shortest string containing every fragment as a substring | [[shortest-common-superstring]] |
| Overlap graph | directed graph whose edges are suffix-prefix overlaps between fragments | [[overlap-graph]] |
| String graph | overlap graph after contained reads and transitive edges are removed | [[string-graph]] |
| Density peak | point with high local density and large distance to any higher-density point | [[density-peak-clustering]] |

## Key formulas

```
P(A ∪ B)  = P(A) + P(B) − P(A ∩ B)
P(X | Y)  = P(X ∩ Y) / P(Y)
P(X | Y)  = P(Y|X) P(X) / P(Y),     P(Y) = P(Y|X)P(X) + P(Y|¬X)P(¬X)
P(x, π)   = a_{0,π₁} ∏(i) e_{π_i}(x[i]) a_{π_i,π_(i+1)}
S(x)      = Σ(i) log( a⁺ / a⁻ )
P(π_i=q|x) = F_x(q,i) B_x(q,i) / P(x)
E_fe(S_w) = Σ_(i,j in S_w) α(w[i],w[j])
id(z,u)   = 100 * (1 - edist(z,u) / ((|z|+|u|)/2))
d_max     = (1 - θ/100) * ((|z|+|u|)/2)
LF(i)     = count[bwt[i]] + R_bwt(bwt[i], i) - 1
rho_g(x)  = Σ_y exp(-(d(x,y)/d_c)^2)
delta(x)  = min{d(x,y) : rho(y) > rho(x)}  unless x has maximum rho
```

## Complexities

| Algorithm / structure | Time | Space | Page |
|---|---|---|---|
| MLE from counts | `O(n)` | `O(\|Σ\|)` | [[maximum-likelihood-estimation]] |
| EM, per iteration (`m=1` shortcut) | `O(n)` | `O(n)` | [[expectation-maximization]] |
| Markov chain evaluation of `P(x)` | `O(ℓ)` | `O(\|S\|²)` | [[markov-chain]] |
| Log-odds scoring | `O(ℓ)` | `O(\|S\|²)` | [[log-odds-score]] |
| Viterbi | `O(\|Q\|²·ℓ)` | `O(\|Q\|·ℓ)` | [[viterbi-algorithm]] |
| Forward | `O(\|Q\|²·ℓ)` | `O(\|Q\|·ℓ)` | [[forward-algorithm]] |
| Backward | `O(\|Q\|²·ℓ)` | `O(\|Q\|·ℓ)` | [[backward-algorithm]] |
| Posterior decoding | `O(\|Q\|²·ℓ)` | `O(\|Q\|·ℓ)` | [[posterior-decoding]] |
| Baum–Welch, per iteration | `O(\|Q\|²·Σ_j\|x_j\|)` | `O(\|Q\|·max\|x_j\|)` | [[baum-welch-algorithm]] |
| Nussinov RNA folding | `O(n^3)` | `O(n^2)` | [[nussinov-algorithm]] |
| Loop-aware RNA folding | `O(n^4)` unrestricted | `O(n^2)` | [[rna-loop-energy-dp]] |
| Unit edit distance DP | `O(mn)` | `O(mn)` | [[unit-edit-distance]] |
| Lazy edit distance | up to `O(mn)`; often band-sized | up to `O(mn)` | [[lazy-edit-distance]] |
| Output-sensitive edit distance | `O(m+n+e^2)` | `O(e)` distance only | [[output-sensitive-edit-distance]] |
| Match chaining, simple DP | `O(b^2)` | `O(b)` | [[match-chaining-algorithm]] |
| Match chaining, dictionary sweep | `O(b log b)` | `O(b)` | [[match-chaining-algorithm]] |
| Suffix tree storage | construction-dependent | `O(n)` | [[suffix-tree]] |
| McCreight construction | `O(n)` | `O(n)` | [[mccreight-suffix-tree-construction]] |
| Exact suffix-tree search | `O(|P|+z)` | `O(1)` plus output | [[suffix-tree-exact-matching]] |
| Approximate suffix-tree search | exponential in `k` in the simple recursion | recursion depth | [[suffix-tree-approximate-matching]] |
| Minimal unique substrings | `O(n+z)` | `O(n)` | [[minimal-unique-substring]] |
| MUM enumeration | `O(|S|+|S'|)` | `O(|S|+|S'|)` | [[maximal-unique-match]] |
| Minimum APSPM | `O(n+z)` | `O(n)` | [[all-pairs-suffix-prefix-matching]] |
| Maximum APSPM | `O(n+k^2)` | `O(n)` | [[all-pairs-suffix-prefix-matching]] |
| Maximal repeats | `O(n+z)` | `O(n)` | [[maximal-repeat-enumeration]] |
| Enhanced suffix array storage | `O(n)` to generate from ST | `O(n)` | [[enhanced-suffix-array]] |
| Suffix-array exact matching | `O(m log n + z)` | `O(1)` plus output | [[suffix-array-pattern-matching]] |
| q-gram suffix-array construction | worst `O(n+r^q+r^q n)` | `O(n+r^q)` | [[q-gram-suffix-array-construction]] |
| BWT construction from `SUF` | `O(n)` | `O(n)` | [[burrows-wheeler-transform]] |
| FM-index backward search | `O(m)` with constant-time rank | `O(1)` extra | [[fm-index-backward-search]] |
| Locate FM-index hit with period `d` | `O(d)` LF steps | `O(n/d)` samples | [[fm-index]] |
| Greedy assembly after overlap graph | `O(ell+n^2)` in notes | `O(n^2)` | [[greedy-fragment-assembly]] |
| Density-peak clustering | `O(n^2 m)` for distances plus `O(n^2)` | `O(n^2)` | [[density-peak-clustering]] |

## Must-know derivations and proofs

1. **MLE of the exponential rate**: `L = λⁿe^(−λnt̄)` → log → differentiate → `λ = 1/t̄`.
   ([[maximum-likelihood-estimation]])
2. **Casino posterior**: `P(D_ld|[6,6,6]) ≈ 0.21` despite a 27× likelihood ratio.
   ([[prior-posterior-probability]])
3. **EM parameter updates** with responsibilities as weights. ([[expectation-maximization]])
4. **Viterbi recurrence** plus traceback, and its log-space form.
   ([[viterbi-algorithm]], [[log-space-computation]])
5. **Forward vs Backward vs Viterbi** — same DP, `max` vs `Σ`, prefix vs suffix.
   ([[hmm-tables-comparison]])
6. **Posterior decoding** as `F·B/P(x)`, and why `π̂` may not be a valid path.
   ([[posterior-decoding]])
7. **Baum–Welch expected counts** from `F` and `B`. ([[baum-welch-algorithm]])
8. **Two-model threshold** (exercise 1.1): cancel `C(ℓ,k)`, take logs, solve for `k`.
   ([[sheet-01]])
9. **Posterior as a sigmoid** (exercise 2.1): `exp(−log(1+exp(ŷ−p̂)))`. ([[sheet-02]])
10. **Nussinov recurrence**: pair ends, drop left, drop right, or split interval. ([[nussinov-algorithm]])
11. **Loop-energy runtime jump**: unrestricted interior loops give the `O(n^4)` term. ([[rna-loop-energy-dp]])
12. **Unit edit-distance recurrence** and neighbour-difference property. ([[unit-edit-distance]], [[lazy-edit-distance]])
13. **Output-sensitive fronts**: generate diagonals `-d,-d+2,...,d` and stop at `front(n-m,d)=m`. ([[output-sensitive-edit-distance]])
14. **OTU threshold conversion**: `id≥θ` iff `edist≤(1-θ/100)ℓ`. ([[operational-taxonomic-unit]])
15. **Match chaining recurrence** over predecessors `f<<f'`. ([[match-chaining-algorithm]])
16. **Suffix-tree size proof**: sentinel gives `n+1` leaves; every internal node branches, so
    internal nodes are at most leaves minus one. ([[suffix-tree]])
17. **McCreight linearity**: suffix links plus `ScanPrefix` charge all rescanned characters only
    constantly often. ([[mccreight-suffix-tree-construction]])
18. **MUM tests**: exactly two leaves below a branching node in `ST(S#S')`, one from each genome,
    plus different left contexts. ([[maximal-unique-match]])
19. **APSPM node characterisation**: terminal edge for `s_i` and whole leaf for `s_j` at the same
    path label imply suffix-prefix overlap. ([[all-pairs-suffix-prefix-matching]])
20. **Maximal repeat annotation**: right-maximal from branching, left-maximal from different
    preceding characters. ([[maximal-repeat-enumeration]])
21. **Suffix-array interval**: two binary searches return first and last suffix ranks prefixed by
    `P`; occurrences are `SUF[l..r]`. ([[suffix-array-pattern-matching]])
22. **LF mapping**: equal occurrence rank of a character links last and first columns, enabling
    text reconstruction and backward search. ([[burrows-wheeler-transform]], [[fm-index-backward-search]])
23. **Assembly as SCS**: for containment-free fragments, maximising path overlap minimises
    superstring length, but exact SCS is NP-hard. ([[shortest-common-superstring]])
24. **Greedy assembly invariant**: accepted edges always form vertex-disjoint paths because
    in-degree, out-degree and union-find cycle tests are enforced. ([[greedy-fragment-assembly]])
25. **Density-peak assignment**: processing in decreasing `rho` makes every nearest denser
    neighbour already assigned. ([[density-peak-clustering]])

## Exam traps

> [!warning]
> - Axiom 3 needs **disjoint** events; otherwise subtract `P(A∩B)`. ([[probability-basics]])
> - High likelihood ≠ high posterior. ([[prior-posterior-probability]])
> - EM and Baum–Welch converge only to a **local** maximum — restart.
> - `σ_r'` uses the **new** mean `μ_r'`; denominators are `Σ r_i`, not `n`. ([[mixture-model]])
> - Markov chain: rows of `A` sum to 1, columns do not.
> - Markov chain states **are** the symbols; HMM states are hidden.
> - Backward's emission factor is `e_k(x[i+1])` — shifted by one versus Forward.
> - Normalise the log-odds score by segment length.
> - Posterior decoding can return an **invalid** path; Viterbi cannot.
> - `log` commutes with `max` but not with `+` — Forward needs log-sum-exp.
> - Unseen events give probability 0 or undefined estimates; use [[pseudocounts]].
> - RNA pair chemistry is not enough; minimum loop length and no-crossing still apply.
> - In RNA MFE, minimise energy; more negative is better.
> - `front(h,d)` uses `h=j-i`; parity mistakes create impossible diagonals.
> - OTU identity thresholds become **upper** bounds on edit distance.
> - Match chaining needs strict order in both sequences.
> - Build suffix trees on `S$`; omitting `$` breaks the leaf/suffix bijection.
> - A substring may end inside an edge; node and edge locations are different.
> - MUMs require uniqueness in both genomes, not just one long exact match.
> - Minimum APSPM and maximal repeat bounds must include output size `z`.
> - The notes use `$` as lexicographically largest for enhanced suffix arrays.
> - In suffix arrays, ranks and text positions are different objects.
> - BWT rank counts equal-character occurrences; row number alone is wrong.
> - FM-index returns an interval before it returns genomic coordinates.
> - Greedy assembly can fail even when every accepted edge is locally best.
> - High `delta` with low `rho` is more likely an outlier than a main density peak.

---

# GSA — Foundations of Sequence Analysis

## GSA definitions

| Term | One-line definition | Page |
|---|---|---|
| Hamming distance | positionwise mismatch count; defined only for `\|u\| = \|v\|` | [[hamming-distance]] |
| Alignment | a sequence of aligned symbol/gap pairs spelling `u` and `v`; a path in the edit graph | [[alignment]] |
| Edit distance | minimum cost `edist_δ(u,v)` over all alignments | [[cost-function]] |
| Levenshtein distance | edit distance under unit cost | [[cost-function]] |
| Edit graph `G(u,v)` | weighted DAG whose source-to-sink paths are exactly the alignments | [[edit-graph]] |
| Local similarity `loc_σ` | max score over all substring pairs | [[local-similarity]] |
| LCS | longest sequence that is a subsequence of both inputs | [[longest-common-subsequence]] |
| `mmdist(v,u)` | size of the minimal partition of `v` into substrings of `u`; asymmetric | [[maximal-matches-distance]] |
| q-gram | substring of length `q`; the unit of alignment-free comparison | [[q-gram]] |
| q-gram profile `P_{u,q}` | occurrence count of every `w ∈ A^q` in `u` | [[q-gram]] |
| `qgdist` | `L1` distance between q-gram profiles; not a metric | [[q-gram-distance]] |
| Jaccard index | `\|U ∩ V\| / \|U ∪ V\|` on q-gram sets | [[jaccard-index]] |
| Sketch | the `s` smallest hash values of a sequence's q-gram set | [[minhash-sketch]] |
| Blast hit | pair `(i,j)` with `score_σ(u[i..i+q-1], w[j..j+q-1]) >= k` | [[blast]] |
| MSP | the sequence pair produced by BLAST's ungapped extension | [[blast]] |
| E-value | expected number of alignments scoring at least as well by chance | [[karlin-altschul-statistics]] |
| PSSM | position-specific scoring matrix built from a sequence family | [[position-specific-scoring-matrix]] |
| Crosspoint | the row where an optimal path crosses a fixed column; Hirschberg's pivot | [[hirschberg-linear-space-alignment]] |
| Sum-of-pairs score | MSA cost as the sum over all induced pairwise alignments | [[multiple-sequence-alignment]] |
| Additive distance | distance realised exactly by path lengths in some edge-weighted tree | [[phylogenetic-tree]] |
| Ultrametric | additive *and* satisfying the three-point condition; a molecular clock | [[ultrametric]] |

## GSA key formulas

| Name | Formula | Page |
|---|---|---|
| Length lemma | `max{m,n} <= h <= m+n` for any alignment length `h` | [[alignment]] |
| Edit-distance DP | `E(i,j) = min{ E(i-1,j-1)+δ(u_i→v_j), E(i-1,j)+δ(u_i→ε), E(i,j-1)+δ(ε→v_j) }` | [[edit-distance-dp]] |
| Smith–Waterman | `α(i,j) = max{ 0, α(i-1,j-1)+σ(u_i,v_j), α(i-1,j)+σ(u_i,−), α(i,j-1)+σ(−,v_j) }` | [[smith-waterman]] |
| LCS recurrence | `LCS(i,j) = LCS(i-1,j-1)+1` if `u_i = v_j`, else `max{LCS(i-1,j), LCS(i,j-1)}` | [[longest-common-subsequence]] |
| LCS ↔ edit distance | `lcslen = (m + n − edist)/2` | [[longest-common-subsequence]] |
| `mmm` metric | `log_2( (mmdist(u,v)+1)·(mmdist(v,u)+1) )` | [[maximal-matches-distance]] |
| q-gram code | `code(w) = Σ_{i=1}^{q} code(w[i])·r^{q-i}` | [[q-gram]] |
| Rolling update | `code(xc) = (code(ax) − code(a)·r^{q-1})·r + code(c)` | [[q-gram]] |
| q-gram distance | `qgdist(u,v) = Σ_{w ∈ A^q} \|P_{u,q}(w) − P_{v,q}(w)\|` | [[q-gram-distance]] |
| Match count | `MC(i,j) = MC(i-1,j-1) − early_match + new_match` | [[match-count]] |
| Mash distance | `MD = −(1/q)·ln( 2J_est / (J_est + 1) )`, and `1` if `J_est = 0` | [[mash-distance]] |
| Poisson match | `P_λ(matching q-gram) = e^{−qd}` | [[mash-distance]] |
| ANI | `(1/\|SLA\|)·Σ_A ( 1 − 2δ(A)/(\|A.u\| + \|A.v\|) )` | [[average-nucleotide-identity]] |
| FASTA score | `score_fasta(u,w) = max_d hotsp(u,w,d)` | [[fasta-database-search]] |
| Normalized score | `S' = (λS − ln K)/ln 2` | [[karlin-altschul-statistics]] |
| E-value | `E = mn / 2^{S'}` | [[karlin-altschul-statistics]] |
| Expected exact matches | `m·n·p^ℓ(1−p)` | [[alignment-significance]] |
| Expected k-LMAs | `m·n·C(ℓ,k)·p^{ℓ−k}(1−p)^{k+2}` | [[alignment-significance]] |
| PSSM look-ahead | prune when the partial score misses `th_d = th − σ_d` | [[pssm-lookahead-search]] |
| Star-alignment bound | `cost(star) / cost(optimal) <= 2 − 2/k` | [[star-alignment]] |

## GSA complexities

| Problem | Time | Space | Page |
|---|---|---|---|
| Edit distance, full matrix | `O(mn)` | `O(mn)` | [[edit-distance-dp]] |
| Edit distance, cost only | `O(mn)` | `O(min{m,n})` | [[edit-distance-dp]] |
| All optimal alignments | `O(mn + z)` | `O(mn)` | [[edit-distance-dp]] |
| Alignment in linear space | `O(mn)` | `O(m+n)` | [[hirschberg-linear-space-alignment]] |
| Affine gap costs | `O(mn)` | `3·O(mn)` | [[gotoh-affine-gap-alignment]] |
| Arbitrary gap costs | `O(mn(m+n))` | `O(mn)` | [[gap-cost-model]] |
| Naive exact matching | `O(nm)` | `O(1)` | [[naive-exact-matching]] |
| `mmdist` via suffix tree | `O(\|A\|(m+n))` | `O(m)` | [[maximal-matches-distance]] |
| q-gram distance | `O(m + n + r^q)` | `O(m + n + r^q)` | [[q-gram-distance]] |
| Match count, all pairs | `O(mn)` | `O(mn)` | [[match-count]] |
| Weighted substring matching | `O(\|S\|)` | `O(1)` | [[weighted-substring-matching]] |
| Jaccard via radix sort + merge | `O(m+n)` | `O(m+n)` | [[jaccard-index]] |
| MinHash sketch | `O(m(q + log s))` | `O(s)` | [[minhash-sketch]] |
| `J_est` for one pair | `O((m+n)(q + log s) + s)` | `O(s)` | [[minhash-sketch]] |
| All-against-all, `k` sequences | `O(n(q + log s) + k²s)` | `O(ks)` | [[minhash-sketch]] |
| FASTA, one database sequence | `O(r^q + m + n + Σ_d hotsp)` | `O(n + r^q)` | [[fasta-database-search]] |
| BLAST, one database sequence | `O(\|u\| + z)` after query preprocessing | `O(r^q)` | [[blast]] |
| Exact MSA, `k` sequences | `O(z·f(k)·2^k)` | exponential in `k` | [[multidimensional-dp-msa]] |
| Star alignment | polynomial; `2 − 2/k` approximation | — | [[star-alignment]] |
| Four-point condition test | `O(n⁴)` | `O(n²)` | [[phylogenetic-tree]] |
| Ultrametric recognition | `O(n³)` | `O(n²)` | [[ultrametric]] |
| UPGMA | `O(n²)` | `O(n²)` | [[upgma]] |

## GSA must-know derivations

- **Alignments are paths.** Build `G(u,v)`, show every path spells an alignment and conversely,
  conclude that minimum cost = shortest path, and that the DAG's topological order makes a double
  loop sufficient. — [[edit-graph]]
- **`mmdist <= edist`.** Turn an alignment into a partition: submatches become match runs, marked
  characters become edit operations. — [[maximal-matches-distance]]
- **`qgdist/(2q) <= edist`.** One edit operation changes at most `q` q-grams in each sequence. —
  [[q-gram-distance]]
- **Mash distance from the Poisson model.** `P(match) = e^{−qd}`, substitute into
  `J ≈ e^{−qd}/(2 − e^{−qd})`, invert for `d`. — [[mash-distance]]
- **Hirschberg.** Optimal path crosses the middle column at some crosspoint; forward scan to the
  middle, backward scan from the end, add the two rows, take the argmin, recurse on both halves.
  Space is two rows; time doubles at worst, so still `O(mn)`. — [[hirschberg-linear-space-alignment]]
- **Star alignment `2 − 2/k`.** Triangle inequality on the centre sequence bounds every induced
  pairwise cost; sum over all pairs. — [[star-alignment]]

## GSA exam traps

> [!warning]
> - Hamming distance needs equal lengths; edit distance does not.
> - `mmdist` is **asymmetric** — `mmdist(u,v) != mmdist(v,u)`. Only `mmm` is a metric.
> - `qgdist` is **not** a metric: distinct sequences can have identical q-gram profiles.
> - A filter bound only lets you **discard**; it never confirms similarity.
> - Smith–Waterman's extra `0` is what makes it local — and the traceback starts at the matrix
>   maximum, not at `(m,n)`, and stops at the first `0`.
> - Approximate matching frees the **start** (`D(0,j) = 0`) but not the end.
> - Affine gap costs need **three** matrices; a single matrix cannot tell "opened a gap" from
>   "extended one".
> - In FASTA the diagonal is `d = j − i`; the lecture's own pages swap the roles of `u` and `w`
>   and flip the sign. State your convention in the exam.
> - BLAST seeds are **scored**, not exact — that is the whole difference from FASTA.
> - A sketch estimates Jaccard; it does not improve asymptotic *time* over q-gram distance, only
>   space.
> - Hirschberg saves **space**, not time; it is still `O(mn)`.
> - Sum-of-pairs is not a metric on alignments, and the optimal MSA is not the union of optimal
>   pairwise alignments.
> - UPGMA assumes a molecular clock (ultrametric); Neighbor Joining only assumes additivity. Using
>   UPGMA on non-ultrametric data gives the wrong topology, not just wrong lengths.

## Related

- [[overview]] — unit map and dependency graph.
- [[index]] — full page catalog.
- [[fsa-fragenkatalog]] — the GSA exam question catalogue, 72 questions with answers.
