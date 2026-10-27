---
title: Index
type: meta
status: draft
updated: 2026-10-15
---

# Index

Catalog of every page in this wiki. See [[overview]] for the module arc and [[log]] for history.
Conventions live in `CLAUDE.md` at the repository root.

The wiki covers **two modules**. Pages carry a `module:` frontmatter key (`GIK` or `GSA`); GIK
pages predate the key and are implicitly `module: GIK`.

- **GIK — Genome Informatics**, units 1–6. Probability, HMMs, RNA folding, string indexes,
  assembly, clustering.
- **GSA — Foundations of Sequence Analysis** (Grundlagen der Sequenzanalyse), parts 1–3 plus an
  exam question catalogue. Edit distance, alignment, alignment-free comparison, database search,
  multiple alignment, phylogeny.

---

# GIK — Genome Informatics

### Meta
- [[overview]] - the GIK module at a glance: units, topic arc, dependency graph. *(draft)*
- [[exam-prep]] - running cheat sheet: definitions, complexities, must-know derivations. *(draft)*
- [[log]] - chronological record of ingests, queries and lint passes.

### Sources
- [[gik_1_2]] - units 1-2 plus sheets 1/2: probability, MLE, Bayes, EM, Markov chains, HMMs. *(26 pages, solid)*
- [[gik_3_4]] - units 3-4 plus sheets 3/4: RNA folding, edit distance, OTUs, match chaining. *(36 pages, solid)*
- [[gik_5]] - unit 5 plus sheet 5: suffix trees, exact/approximate matching, uniqueness, overlaps, repeats, enhanced suffix arrays. *(45 pages, solid)*
- [[gik_6]] - unit 6 plus sheet 6: suffix arrays, BWT/FM-index, assembly graphs, density-peak clustering. *(32 pages, solid)*

### Concepts
- [[probability-basics]] - events, sample space, the three axioms, conditional and joint probability. *(unit 1, solid)*
- [[probabilistic-model]] - a model producing different outcomes with different probabilities. *(unit 1, solid)*
- [[sigmoid-function]] - sigmoid and exponential function shapes and their parameters. *(unit 1, solid)*
- [[maximum-likelihood-estimation]] - choose `Theta` maximising `P(D|Theta)`; coin and traffic-delay examples. *(unit 1, solid)*
- [[prior-posterior-probability]] - Bayes' theorem; the casino dice worked through. *(unit 1, solid)*
- [[numerical-integration-midpoint]] - midpoint rule for the EM E-step integral. *(unit 1, solid)*
- [[cpg-island]] - CpGs rarer than expected; the motivating problem of unit 2. *(unit 2, solid)*
- [[log-odds-score]] - `log P(x|+)/P(x|-)`, length-normalised, to classify a segment. *(unit 2, solid)*
- [[hmm-three-problems]] - decoding, evaluation, learning, and which algorithm solves each. *(unit 2, solid)*
- [[hmm-tables-comparison]] - `V`, `F` and `B` side by side. *(unit 2, solid)*
- [[posterior-decoding]] - `F*B/P(x)`; the alternative to Viterbi. *(unit 2, solid)*
- [[hmm-parameter-training]] - topology vs parameters; counting vs the NP-hard case. *(unit 2, solid)*
- [[pseudocounts]] - artificial counts preventing zero and undefined probabilities. *(unit 2, solid)*
- [[log-space-computation]] - products become sums of logs; underflow avoidance. *(units 1-2, solid)*
- [[rna-secondary-structure]] - non-crossing allowed RNA base-pair sets with loop-length constraints. *(units 3-4, solid)*
- [[rna-secondary-structure-representation]] - graph, mountain, Vienna dot-bracket, circle and tree encodings. *(units 3-4, solid)*
- [[unit-edit-distance]] - unit-cost insertion/deletion/substitution DP and local properties. *(units 3-4, solid)*
- [[operational-taxonomic-unit]] - identity-threshold sequence clusters and edit-distance cutoffs. *(unit 4, solid)*
- [[anchor-based-sequence-comparison]] - compute matches, chain anchors, align between anchors. *(units 4-5, solid)*
- [[match-chaining]] - maximum-score collinear non-overlapping anchor chain problem. *(units 4-5, solid)*
- [[supermaximal-repeat]] - maximal repeats not contained in any longer maximal repeat. *(unit 6, solid)*
- [[fragment-assembly]] - overlap-layout-consensus reconstruction from shotgun fragments. *(unit 6, solid)*
- [[shortest-common-superstring]] - idealised assembly as a minimum-length superstring problem. *(unit 6, solid)*

### Models
- [[mixture-model]] - two 1-d normals, with and without known source. *(unit 1, solid)*
- [[markov-chain]] - each symbol conditioned on its predecessor; plus/minus transition models. *(unit 2, solid)*
- [[hidden-markov-model]] - quadruple `(Sigma,Q,A,e)`; symbols observed, states hidden. *(unit 2, solid)*
- [[rna-free-energy-model]] - independent-pair and loop-aware energy models for RNA folding. *(units 3-4, solid)*

### Algorithms
- [[expectation-maximization]] - iterative MLE with hidden variables; E-step and M-step. *(unit 1, solid)*
- [[viterbi-algorithm]] - DP for the most probable path; `O(|Q|^2*l)`. *(unit 2, solid)*
- [[forward-algorithm]] - DP over prefixes for `P(x|M)`. *(unit 2, solid)*
- [[backward-algorithm]] - DP over suffixes for `P(x|M)`. *(unit 2, solid)*
- [[baum-welch-algorithm]] - EM for HMMs; expected counts from `F` and `B`. *(unit 2, solid)*
- [[nussinov-algorithm]] - interval DP for RNA MFE under independent base-pair energies. *(unit 3, solid)*
- [[rna-loop-energy-dp]] - loop-aware RNA folding DP with hairpin, helix, bulge and interior terms. *(units 3-4, solid)*
- [[lazy-edit-distance]] - stack/memo evaluation of only demanded edit-distance cells. *(unit 3, solid)*
- [[output-sensitive-edit-distance]] - front-based edit distance in `O(m+n+e^2)`. *(units 3-4, solid)*
- [[match-chaining-algorithm]] - quadratic and `O(b log b)` algorithms for anchor chaining. *(unit 4, solid)*
- [[mccreight-suffix-tree-construction]] - linear-time suffix-tree construction using suffix links. *(unit 5, solid)*
- [[suffix-tree-exact-matching]] - top-down pattern traversal and leaf reporting. *(unit 5, solid)*
- [[suffix-tree-approximate-matching]] - bounded edit search on suffix-tree locations. *(unit 5, solid)*
- [[minimal-unique-substring]] - enumerate first unique extensions from repeated nodes. *(unit 5, solid)*
- [[maximal-unique-match]] - MUM anchors from `ST(S#S')` with maximality tests. *(unit 5, solid)*
- [[all-pairs-suffix-prefix-matching]] - all directed overlaps using terminal edges and whole leaves. *(unit 5, solid)*
- [[maximal-repeat-enumeration]] - bottom-up annotation algorithm for exact maximal repeats. *(unit 5, solid)*
- [[shortest-absent-word]] - BFS for the shortest alphabet word absent from a string. *(unit 5, solid)*
- [[suffix-array-pattern-matching]] - two binary searches for the suffix-array pattern interval. *(unit 6, solid)*
- [[q-gram-suffix-array-construction]] - bucket suffixes by fixed-length prefixes before sorting. *(unit 6, solid)*
- [[fm-index-backward-search]] - BWT interval update from right to left. *(unit 6, solid)*
- [[greedy-fragment-assembly]] - maximum-overlap Hamiltonian-path heuristic with degree/cycle tests. *(unit 6, solid)*
- [[density-peak-clustering]] - choose high-density, well-separated centres and assign by nearest denser point. *(unit 6, solid)*

### Data structures
- [[suffix-tree]] - compact trie of all suffixes of `S$`; linear-space string index. *(unit 5, solid)*
- [[enhanced-suffix-array]] - `SUF` and `LCP` arrays representing suffix-tree order and intervals. *(units 5-6, solid)*
- [[suffix-array]] - lexicographically sorted suffix starts; compact exact-matching index. *(unit 6, solid)*
- [[burrows-wheeler-transform]] - preceding-character permutation supporting LF mapping. *(unit 6, solid)*
- [[fm-index]] - compressed BWT-based full-text index with sampled suffix-array values. *(unit 6, solid)*
- [[overlap-graph]] - directed multigraph of suffix-prefix fragment overlaps. *(unit 6, solid)*
- [[string-graph]] - reduced overlap graph after contained reads and transitive edges are removed. *(unit 6, solid)*

### Formats & tools
*(none yet)*

### Exercises
- [[sheet-01]] - sheet 1: 1.1 two-coin model comparison; 1.3 Viterbi in log space. *(solid)*
- [[sheet-02]] - sheet 2: 2.1 posterior rewritten as a sigmoid. *(draft - only task 2.1 present)*
- [[sheet-03]] - sheet 3: RNA structure counting, edit-distance properties, front tracing. *(solid)*
- [[sheet-04]] - sheet 4: local match chaining with gap penalties. *(solid)*
- [[sheet-05]] - sheet 5: absent words, suffix-number intervals, suffix-tree tables, enhanced suffix arrays. *(solid)*
- [[sheet-06]] - sheet 6: suffix-array/BWT operations, greedy assembly, density-peak clustering. *(solid)*

### Queries
*(none yet)*

---

# GSA — Foundations of Sequence Analysis

*Grundlagen der Sequenzanalyse.* Parts 1–3 are continuous lecture notes; the Fragenkatalog is the
exam question catalogue. `units: [N]` on these pages means **FSA part N**, not a GIK unit.

### Sources
- [[fsa_part1]] — part 1: edit operations, alignments, edit distance DP, Smith–Waterman, exact matching, overlap alignment, gap costs. *(29 pages, solid)*
- [[fsa_part2]] — part 2: LCS, maximal-match distance, q-grams, MinHash/Mash, FASTA, BLAST. *(26 pages, solid)*
- [[fsa_part3]] — part 3: PSSMs, significance, Hirschberg, multiple alignment, phylogeny. *(30 pages, solid)*
- [[fsa_fragenkatalog]] — the exam question catalogue: 72 questions in §1–§11 with handwritten answers. *(36 pages, solid)*

### Concepts
- [[hamming-distance]] — positionwise mismatch count; defined only for equal-length sequences. *(part 1, solid)*
- [[edit-operation]] — insertion, deletion, replacement (and part 3's swap); edit scripts. *(parts 1, 3, solid)*
- [[alignment]] — the formal alignment object and the length lemma `max{m,n} <= h <= m+n`. *(part 1, solid)*
- [[counting-alignments]] — the `Aligns(m,n)` recurrence, Stanton–Cowan asymptotics, and `Subseq(m,n)`. *(part 1, solid)*
- [[cost-function]] — cost functions, unit cost, Levenshtein distance, the metric properties, the reversal lemma. *(part 1, solid)*
- [[edit-graph]] — `G(u,v)` as a weighted DAG; alignments are source-to-sink paths. *(part 1, solid)*
- [[score-function]] — scores vs costs; BLOSUM62. *(part 1, solid)*
- [[local-similarity]] — `loc_σ`, the quantity Smith–Waterman maximises. *(part 1, solid)*
- [[gap-cost-model]] — linear, affine and arbitrary gap costs; the three-layer edit graph. *(part 1, solid)*
- [[longest-common-subsequence]] — the LCS graph, the `LCS_δ` recurrence, and `lcslen = (m+n−edist)/2`. *(parts 2, 3, solid)*
- [[maximal-matches-distance]] — partitions `Ψ_lr`/`Ψ_rl`, `mmdist`, the `mmm` metric, edit-distance filtering. *(parts 2, 3, solid)*
- [[q-gram]] — q-grams, the profile `P_{u,q}`, integer coding with constant-time rolling update. *(parts 2, 3, solid)*
- [[q-gram-distance]] — `qgdist`, why it is no metric, and the filter bound `qgdist/(2q) <= edist`. *(parts 2, 3, solid)*
- [[match-count]] — `MC(i,j)` and its diagonal incremental recurrence. *(part 2, solid)*
- [[jaccard-index]] — `|U ∩ V|/|U ∪ V|` on q-gram sets; linear-time via radix sort and merge. *(part 2, solid)*
- [[mash-distance]] — the Poisson mutation model and `MD_{q,s}` derived from the Jaccard estimate. *(part 2, solid)*
- [[average-nucleotide-identity]] — the gold standard that Mash distance is validated against. *(part 2, solid)*
- [[karlin-altschul-statistics]] — normalized score `S' = (λS − ln K)/ln 2` and `E = mn/2^{S'}`. *(part 2, solid)*
- [[alignment-significance]] — Bernoulli null model; expected counts of exact matches and k-LMAs. *(part 3, solid)*
- [[multiple-sequence-alignment]] — formal definition, projection/compatibility, sum-of-pairs and consensus costs. *(part 3, solid)*
- [[phylogenetic-tree]] — edge lengths, additivity, the four-point condition. *(part 3, solid)*
- [[ultrametric]] — the molecular clock assumption and ultrametric distance matrices. *(part 3, solid)*

### Models
- [[position-specific-scoring-matrix]] — PSSM built from a sequence family; `score(w,M)`. *(part 3, solid)*

### Algorithms
- [[edit-distance-dp]] — the `E_δ(i,j)` DP, traceback, `O(m)`-space cost-only variant, all alignments in `O(mn+z)`. *(part 1, solid)*
- [[smith-waterman]] — local alignment via the `α_σ` recurrence with the extra `0`. *(part 1, solid)*
- [[approximate-string-matching]] — free start `D_δ(0,j)=0` and the traceback bound. *(part 1, solid)*
- [[naive-exact-matching]] — the naive `O(nm)` scan and the bad-character shift table. *(part 1, solid)*
- [[overlap-alignment]] — the `Ov_δ` DP, four overlap cases and four traceback shapes. *(part 1, solid)*
- [[gotoh-affine-gap-alignment]] — three-matrix affine gap alignment in `O(mn)`. *(part 1, solid)*
- [[weighted-substring-matching]] — `Sol(σ,S,w)` via an `O(|S|)` sliding window. *(part 2, solid)*
- [[minhash-sketch]] — bottom-`s` hash sketches and the Jaccard estimate `J_est`. *(part 2, solid)*
- [[fasta-database-search]] — hot spots, `score_fasta`, the `h_w` bucket table, diagonal runs and their DAG. *(part 2, solid)*
- [[blast]] — five stages: hits, diagonal pairs, ungapped and gapped extension, E-values. *(part 2, solid)*
- [[pssm-lookahead-search]] — threshold look-ahead `th_d = th − σ_d` to prune PSSM scanning. *(part 3, solid)*
- [[hirschberg-linear-space-alignment]] — divide and conquer on crosspoints; `O(mn)` time, `O(m+n)` space. *(part 3, solid)*
- [[multidimensional-dp-msa]] — exact k-dimensional MSA DP in `O(z·f(k)·2^k)`, with pruning. *(part 3, solid)*
- [[star-alignment]] — centre-star heuristic with the `(2 − 2/k)` approximation guarantee. *(part 3, solid)*
- [[progressive-alignment]] — guide trees, progressive and iterative refinement, divide and conquer. *(part 3, solid)*
- [[upgma]] — average-linkage ultrametric tree building in `O(n²)`. *(part 3, solid)*
- [[neighbor-joining]] — additive tree reconstruction without a molecular clock. *(part 3, solid)*

### Exercises
- [[fsa-fragenkatalog]] — hub for the exam question catalogue: how to drill, §1–§11 map, complexity summary. *(solid)*
- [[fsa-fragenkatalog-1]] — §1–§2: edit distance model, DP, edit graph, complexity. *(solid)*
- [[fsa-fragenkatalog-2]] — §3–§5: alignment variants, gap costs, alignment-free comparison. *(solid)*
- [[fsa-fragenkatalog-3]] — §6–§9: significance, Fasta/Blast, PSSMs, linear-space alignment. *(solid)*
- [[fsa-fragenkatalog-4]] — §10–§11: multiple sequence alignment and phylogeny. *(solid)*
