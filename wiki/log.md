---
title: Log
type: meta
status: draft
updated: 2026-10-15
---

# Log

Append-only, newest at the bottom. Entry headers use the prefix `## [YYYY-MM-DD] kind | subject`
so that `grep "^## \[" wiki/log.md | tail -5` returns the recent history.

## [2026-10-15] refactor | wiki initialised
created: `CLAUDE.md`, [[index]], [[overview]], [[exam-prep]], [[log]]
updated: —
notes: scaffold only, no sources ingested. `raw/` contains gik_1_2.pdf, gik_3_4.pdf, gik_5.pdf,
gik_6.pdf. Next step: ingest gik_1_2.pdf.

## [2026-10-15] refactor | added models/ category to the schema
created: `wiki/models/`
updated: `CLAUDE.md`
notes: Markov chains, HMMs and mixture models are models rather than data structures. Added the
`model` page type, a `models/` directory, and a model-specific template section (formal
definition, parameter estimation, model diagram).

## [2026-10-15] ingest | gik_1_2.pdf — Units 1–2: probability, MLE, EM, Markov chains, HMMs
created: [[gik_1_2]], [[probability-basics]], [[probabilistic-model]], [[sigmoid-function]],
[[maximum-likelihood-estimation]], [[prior-posterior-probability]],
[[numerical-integration-midpoint]], [[cpg-island]], [[log-odds-score]], [[hmm-three-problems]],
[[hmm-tables-comparison]], [[posterior-decoding]], [[hmm-parameter-training]], [[pseudocounts]],
[[log-space-computation]], [[mixture-model]], [[markov-chain]], [[hidden-markov-model]],
[[expectation-maximization]], [[viterbi-algorithm]], [[forward-algorithm]],
[[backward-algorithm]], [[baum-welch-algorithm]], [[sheet-01]], [[sheet-02]]
updated: [[index]], [[overview]], [[exam-prep]]
notes: 26 scanned handwritten pages, no text layer — rendered to images and read visually.
Connections worth remembering: exercise 2.1 returns to the sigmoid of p. 1, and exercise 1.3 is
the fix for the Viterbi underflow noted on p. 17. Open questions recorded on [[gik_1_2]]: task
1.2 and any sheet-2 tasks beyond 2.1 are missing from the notes; pseudocounts are listed as
Baum–Welch inputs but omitted from its update formulas; the loaded die differs slightly between
the p. 6 and p. 15 examples.

## [2026-10-15] ingest | gik_3_4.pdf - RNA folding, edit distance, OTUs, match chaining
created: [[gik_3_4]], [[rna-secondary-structure]], [[rna-secondary-structure-representation]], [[rna-free-energy-model]], [[nussinov-algorithm]], [[rna-loop-energy-dp]], [[unit-edit-distance]], [[lazy-edit-distance]], [[output-sensitive-edit-distance]], [[operational-taxonomic-unit]], [[anchor-based-sequence-comparison]], [[match-chaining]], [[match-chaining-algorithm]], [[sheet-03]], [[sheet-04]]
updated: [[index]], [[overview]], [[exam-prep]], [[log]]
notes: 36 scanned handwritten pages, no text layer — rendered and read visually. No contradictions with units 1-2 found; cross-unit DP connections were wired through [[unit-edit-distance]], [[output-sensitive-edit-distance]], and [[match-chaining-algorithm]].

## [2026-10-15] ingest | gik_5.pdf - Suffix trees, MUMs, overlaps, repeats, enhanced suffix arrays
created: [[gik_5]], [[suffix-tree]], [[mccreight-suffix-tree-construction]], [[suffix-tree-exact-matching]], [[suffix-tree-approximate-matching]], [[minimal-unique-substring]], [[maximal-unique-match]], [[all-pairs-suffix-prefix-matching]], [[maximal-repeat-enumeration]], [[shortest-absent-word]], [[enhanced-suffix-array]], [[sheet-05]]
updated: [[index]], [[overview]], [[exam-prep]], [[anchor-based-sequence-comparison]], [[match-chaining]], [[log]]
notes: 45 scanned handwritten pages, no text layer — rendered and read visually. Unit 5 supplies the suffix-tree and suffix-array indexing layer behind the anchors introduced in units 3–4; no contradictions found, but exercise pseudocode on pp. 38 and 41 is dense and was normalised into explicit BFS/DFS procedures.

## [2026-10-15] ingest | gik_6.pdf - Suffix arrays, BWT/FM-index, assembly, clustering
created: [[gik_6]], [[suffix-array]], [[suffix-array-pattern-matching]], [[q-gram-suffix-array-construction]], [[supermaximal-repeat]], [[burrows-wheeler-transform]], [[fm-index]], [[fm-index-backward-search]], [[fragment-assembly]], [[overlap-graph]], [[shortest-common-superstring]], [[greedy-fragment-assembly]], [[string-graph]], [[density-peak-clustering]], [[sheet-06]]
updated: [[index]], [[overview]], [[exam-prep]], [[enhanced-suffix-array]], [[maximal-repeat-enumeration]], [[all-pairs-suffix-prefix-matching]], [[log]]
notes: 32 scanned handwritten pages, no text layer — rendered and read visually. Unit 6 completes the module by extending unit-5 suffix indexes to suffix arrays, BWT/FM-indexes and overlap/string graphs, then closes with density-peak clustering. No contradictions found; [[sheet-06]] is inferred because no explicit printed sheet heading appears in the source.

## [2026-10-15] lint | post-ingest health check (all 6 units)

- Scope: all 71 wiki pages after ingesting gik_1_2, gik_3_4, gik_5 and gik_6.
- Checks run: broken wikilinks, orphan pages, frontmatter presence, required sections,
  pseudocode in algorithm pages, Mermaid diagrams in structural pages, wikilinks leaking
  into Mermaid labels, index coverage.
- Result: 0 broken links, 0 orphans, 0 Mermaid-label wikilinks, 0 pages missing from index.md.
- Fixes applied:
  - Stripped UTF-8 BOMs from 12 unit-5 pages; the BOM prevented Obsidian from parsing their
    YAML frontmatter.
  - Added the missing `## Sources` footer to gik_3_4, gik_5 and gik_6.
  - Corrected "Two themes" to "Three themes" in overview.md.
- Remaining advisory items (not auto-fixed): several unit-1/2 concept pages have no
  `Worked example` section; the open questions recorded on each source page are still open.

## [2026-10-21] lint | pre-ingest pass before the foundations_sequence_analysis series

- Scope: all 71 pages. Added deep checks for Mermaid label syntax (unquoted `()`/`#` in node
  labels), pseudocode house style, markdown table shape and fence balance.
- Fixed (Mermaid parse errors, diagrams would not render in Obsidian):
  - [[suffix-tree-exact-matching]] — `T[Traverse ST(S$)]` → quoted label.
  - [[maximal-unique-match]] — `X[S#S']` → quoted label (`#` starts an entity code).
  - [[minimal-unique-substring]] — colon in an unquoted label → quoted.
- Fixed (template conformance, CLAUDE.md §4):
  - [[hidden-markov-model]], [[markov-chain]] — split the tuple out into `## Formal definition`.
  - [[mixture-model]] — added `## Parameters and how they are estimated`.
  - [[forward-algorithm]] — added `## Invariant / correctness argument` and `## Variants`.
  - [[backward-algorithm]] — added `## Variants`.
  - [[hmm-parameter-training]], [[hmm-tables-comparison]] — added `## Why it matters`.
- Verified clean: 0 broken wikilinks, 0 orphans, 0 ragged tables, 0 unbalanced code fences,
  0 wikilinks inside Mermaid labels, no page over 400 lines.
- Advisory, not auto-fixed: eight purely mathematical concept pages have no `## Complexity`
  table (no algorithmic operation to tabulate); source pages and sheet-01/02 carry no Mermaid.

## [2026-10-21] ingest | foundations_sequence_analysis_part1.pdf — Edit distance, alignment, gap costs

Start of a **second module**: GSA — Foundations of Sequence Analysis (Grundlagen der
Sequenzanalyse). Distinct from GIK; pages carry `module: GSA` and `units: [N]` meaning *FSA part N*.
created: [[fsa_part1]], [[hamming-distance]], [[edit-operation]], [[alignment]],
[[counting-alignments]], [[cost-function]], [[edit-graph]], [[score-function]],
[[local-similarity]], [[gap-cost-model]], [[edit-distance-dp]], [[smith-waterman]],
[[approximate-string-matching]], [[naive-exact-matching]], [[overlap-alignment]],
[[gotoh-affine-gap-alignment]]
updated: [[fragment-assembly]] (GSA cross-reference section), [[unit-edit-distance]] (Related)
notes: 29 scanned handwritten pages, no text layer — rendered to PNG at dpi=110 and read visually.
  No exercises in part 1; it is continuous lecture notes. Eight source inconsistencies flagged
  in-page, incl. swapped index ranges in the overlap criterion (p. 22), the gap-initiation
  constant written both `q` and `g` (pp. 26 vs 28–29), and an `O(mn(m+n))` general-gap DP
  labelled "Needleman–Wunsch" against the usual `O(mn)` usage (p. 26).

## [2026-10-21] ingest | foundations_seqzence_analysis_part2.pdf — Alignment-free comparison, FASTA, BLAST

created: [[fsa_part2]], [[longest-common-subsequence]], [[maximal-matches-distance]], [[q-gram]],
[[q-gram-distance]], [[match-count]], [[jaccard-index]], [[mash-distance]],
[[average-nucleotide-identity]], [[karlin-altschul-statistics]], [[weighted-substring-matching]],
[[minhash-sketch]], [[fasta-database-search]], [[blast]]
notes: 26 scanned pages. The raw filename is misspelled — `foundations_seqzence_analysis_part2.pdf`.
  `raw/` is immutable so the typo stays; the source page records it. Eight source inconsistencies
  flagged, incl. an off-by-one between the `MC(i,j)` window definition and its recurrence (p. 8),
  a bucket-table trace whose prefix sums contradict the written `H` slots (p. 19), and
  `# q-grams = r^q − 1` which is the largest index, not the count (p. 5).

## [2026-10-21] ingest | foundations_sequence_analysis_part3.pdf — PSSMs, linear space, MSA, phylogeny

created: [[fsa_part3]], [[position-specific-scoring-matrix]], [[pssm-lookahead-search]],
[[alignment-significance]], [[hirschberg-linear-space-alignment]], [[multiple-sequence-alignment]],
[[multidimensional-dp-msa]], [[star-alignment]], [[progressive-alignment]],
[[phylogenetic-tree]], [[ultrametric]], [[upgma]], [[neighbor-joining]]
updated: [[edit-operation]] (swap/transposition), [[longest-common-subsequence]]
  (`lcslen = (m+n−edist)/2`), [[q-gram]] (complement coding), [[maximal-matches-distance]]
  (all four partitions), [[q-gram-distance]] (qgdist-vs-edist cases)
notes: 30 scanned pages. UPGMA step 3 on p. 27 is self-contradictory — names cluster
  `{{1,2},3}` but uses height `½ d(3,7)` with node 7 = {4,5}; only "merge {3} with {4,5}" is
  consistent and that reading was used. The symbol `k` is overloaded four ways across part 3.
  The Hirschberg worked trace is marked `Inference`: the source has only schematics, so the trace
  was computed from the lecture's own recurrences.

## [2026-10-21] ingest | foundations_sequence_analysis_fragenkatalog.pdf — Exam question catalogue

created: [[fsa_fragenkatalog]], [[fsa-fragenkatalog]], [[fsa-fragenkatalog-1]],
[[fsa-fragenkatalog-2]], [[fsa-fragenkatalog-3]], [[fsa-fragenkatalog-4]]
notes: 36 pages — pp. 1–9 the printed German questions, pp. 10–36 the handwritten answers.
  72 printed numbers expanding to 75 answered items across §1–§11. Split into a hub plus four
  part pages to respect the 400-line cap. Every answer links to the concept page carrying the
  full treatment, so the catalogue doubles as a topical index into the GSA half of the wiki.
  Coverage gaps recorded (no pages created): elementary graph theory (Q1.17–Q1.19), the Poisson
  distribution itself (Q4.14), binomial coefficients (Q5.3). 14 `> [!question]` callouts mark
  answers that are incomplete, self-contradictory or illegible in the source.

## [2026-10-21] lint | post-ingest pass over the full two-module wiki

- Scope: all 120 pages (71 GIK + 49 GSA). Structural pass, plus a rewritten deep pass.
- Tooling improved after false positives in the pre-ingest run:
  - The wikilink regex now tolerates the escaped pipe used for aliased links inside markdown
    tables, which the previous version reported as 12 broken links. Those were never broken.
  - The pseudocode check no longer flags descriptive `text` blocks that legitimately contain no
    `<-` assignment.
  - The table check strips inline-code spans and escaped pipes before counting columns, which
    removes all 25 previously reported "ragged tables".
  - Mermaid labels are now parsed as real node definitions, with quoted labels excluded, instead
    of by substring search. This is what caught the one genuine error below.
- Fixed:
  - [[suffix-array-pattern-matching]] — `O[positions SUF[l..r]]`: nested `[` in an unquoted
    Mermaid label. A genuine parse error that the pre-ingest heuristic missed.
  - [[forward-algorithm]] — `[[viterbi-algorithm|Viterbi]]` inside a table cell: the unescaped
    pipe split the row into five columns. Escaped to `\|`. Introduced by the pre-ingest pass.
  - [[cost-function]], [[gap-cost-model]], [[alignment-significance]] — added the missing
    `## Why it matters` section required by CLAUDE.md §4.1.
  - [[alignment]], [[counting-alignments]], [[edit-graph]], [[gap-cost-model]] — added
    `## Complexity` tables.
  - Added `[[fsa_part1]]` backlinks to all 15 part-1 content pages, which were otherwise
    reachable only from [[index]].
- Verified clean: 0 broken wikilinks, 0 orphans, 0 ragged tables, 0 unbalanced fences,
  0 wikilinks inside Mermaid labels, 0 Mermaid parse errors, 0 pseudocode style violations,
  no page over 400 lines. 115 solid / 5 draft / 0 stub.
- On representation (the question that prompted this pass): pseudocode and Mermaid are used
  correctly and are the right tools for procedures and for structure. Three gaps remain where a
  different representation would serve better — DP matrices with traceback arrows, which
  markdown tables render but cannot annotate; alignment columns, which want a fixed-width block
  rather than a table; and the `Env_k` trie and UPGMA merge trees, where Mermaid can draw the
  shape but not the numbers on it. Proposed options recorded in the session report; none applied,
  as they would change conventions wiki-wide.
- Advisory, not auto-fixed: eleven purely mathematical concept pages still have no
  `## Complexity` table — there is no algorithmic operation to tabulate and inventing one
  would be fabrication. Source pages carry no Mermaid by design.
