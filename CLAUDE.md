# Genome Informatics Wiki — Schema

This repository is an **LLM-maintained wiki** for the *Genome Informatics* module (GIK) at
Universität Hamburg (UHH). The lecture and the exercises are taught as one combined unit, and
the wiki treats them that way: every concept page should carry both the theory and the
exercise-level mechanics (worked steps, pseudocode, pitfalls).

**Language: English only.** Source PDFs may contain German; translate terminology into English
and keep the German term in parentheses on first mention, e.g. `suffix array (Suffixarray)`.

**Division of labour**
- The user curates sources into `raw/`, asks questions, and reviews.
- You (the LLM) own everything under `wiki/`. You write it; the user reads it.
- `raw/` is **immutable**. Never edit, rename, move, or delete anything there.

---

## 1. Directory layout

```
raw/                      # immutable sources: gik_<units>.pdf (lecture + exercises combined)
wiki/
  index.md                # catalog of every page (content-oriented)
  log.md                  # append-only chronological record
  overview.md             # the module at a glance: units, arc, dependency graph
  exam-prep.md            # running cheat sheet: definitions, complexities, must-know proofs
  sources/                # one page per ingested source PDF
    gik_1_2.md
  concepts/               # the main body: one page per concept/problem
    suffix-array.md
  models/                 # probabilistic/computational models (Markov chain, HMM, mixture model)
    hidden-markov-model.md
  algorithms/             # one page per named algorithm
    needleman-wunsch.md
  data-structures/        # one page per data structure
    fm-index.md
  formats-tools/          # file formats and tools (FASTQ, SAM/BAM, VCF, BWA, samtools, ...)
    sam-bam.md
  exercises/              # one page per exercise sheet / task group
    sheet-01.md
  queries/                # filed answers to the user's questions
    2026-10-15-affine-vs-linear-gaps.md
```

Create directories lazily — only when the first page of that kind is written.

## 2. File and link conventions

- Filenames: `kebab-case.md`, singular (`suffix-array.md`, not `suffix-arrays.md`).
- Links: Obsidian wikilinks, `[[suffix-array]]`, or `[[suffix-array|suffix arrays]]` with an alias.
  Never use relative paths in links.
- Every page starts with YAML frontmatter (see §3).
- Every page must be reachable: linked from `index.md` **and** from at least one other
  content page. No orphans.
- Cite sources inline as `(gik_3_4, slide 12)` or `(gik_5, exercise 2b)`. Every non-obvious
  claim gets a citation. If something is your own inference rather than the lecture's claim,
  mark it `> [!note] Inference` in a callout.

## 3. Frontmatter

```yaml
---
title: Suffix Array
type: concept            # source | concept | model | algorithm | data-structure | format-tool | exercise | query | meta
units: [3, 4]            # GIK lecture units this came from
sources: [gik_3_4]       # raw PDFs that contributed
tags: [indexing, string-matching, exam-relevant]
status: stub             # stub | draft | solid
updated: 2026-10-15
---
```

`status` meanings: `stub` = placeholder with links only; `draft` = content present, gaps remain;
`solid` = complete enough to revise from for the exam.

## 4. Page templates

### 4.1 Concept / model / data-structure page

```markdown
## Definition
One-paragraph precise definition. German term in parentheses on first mention.

## Why it matters
The biological or computational problem it solves. Link to [[...]] motivating pages.

## Construction / Mechanism
Mermaid diagram or worked micro-example on a tiny string/sequence (≤10 symbols).

## Pseudocode
(see §5)

## Complexity
| Operation | Time | Space |

## Worked example
Small, fully traced, numbers shown. This is what makes the page exam-useful.

## Pitfalls & exam traps
Bullet list — off-by-ones, 0- vs 1-indexing, assumptions that silently break.

## Related
[[...]] — one line each explaining the relationship, not a bare link dump.

## Sources
- gik_3_4, slides 10–24; exercise 2
```

### 4.2 Algorithm page

Same as above plus an **Invariant / correctness argument** section and a
**Variants** section (e.g. global vs local vs semi-global alignment).

A **model** page additionally carries a **Formal definition** section (the tuple and what each
component is), a **Parameters and how they are estimated** section, and a **Model diagram**
(Mermaid state graph where the model has states).

### 4.3 Source page (`wiki/sources/gik_X.md`)

```markdown
## Scope
Units covered, lecture topics, exercise tasks.

## Key takeaways
5–12 bullets, each linking to the concept page that carries the detail.

## Lecture content
Section-by-section condensation. No slide-by-slide transcription — synthesize.

## Exercises
Per task: the problem, the method, the answer, and the generalizable lesson.

## Pages created / updated by this ingest
- created: [[suffix-array]], [[bwt]]
- updated: [[overview]], [[exam-prep]]

## Open questions / unclear in source
```

## 5. Pseudocode and diagrams (required)

**Pseudocode** for every algorithm and for any exercise procedure with more than two steps.
Fenced as ```text, uniform style:

```text
ALGORITHM NeedlemanWunsch(A[1..n], B[1..m], s, d)
  for i = 0 to n:  D[i][0] <- i * d
  for j = 0 to m:  D[0][j] <- j * d
  for i = 1 to n:
    for j = 1 to m:
      D[i][j] <- max( D[i-1][j-1] + s(A[i], B[j]),
                      D[i-1][j]   + d,
                      D[i][j-1]   + d )
  return D[n][m]
```

Rules: `<-` for assignment, 1-based indexing unless the lecture uses 0-based (then say so
explicitly at the top of the block), explicit loop bounds, no language-specific syntax.

**Mermaid diagrams** wherever structure, flow, or relationships are involved — pipelines,
DP traceback paths, tree/graph structures, assembly graphs, decision flows. Fence as ```mermaid.
Prefer `graph LR`/`graph TD` for structure, `flowchart` for procedures, `sequenceDiagram` for
protocol-like exchanges. Keep them under ~15 nodes; split rather than sprawl.

> Never put `[[wikilinks]]` inside Mermaid node labels — Mermaid renders them as literal text
> and Obsidian does not resolve them. Use the plain page name in the label and put the real
> links in the surrounding prose or the Related section.

```mermaid
graph LR
  R[Reads FASTQ] --> QC[QC / trimming]
  QC --> AL[Alignment BWA]
  AL --> S[SAM/BAM]
  S --> V[Variant calling]
  V --> VCF[VCF]
```

Do not use ASCII art where Mermaid works. Do use plain tables for DP matrices — Mermaid is bad
at those.

## 6. Operations

### Ingest
Trigger: *"ingest raw/gik_X.pdf"*.

1. Read the PDF end to end. Read `wiki/index.md` and the pages it says are related.
2. Report back: unit scope, the 5–10 concepts found, which are new vs. already covered, and
   which existing pages will change. **Pause for the user's confirmation and emphasis.**
3. Write `wiki/sources/gik_X.md`.
4. Create/update concept, algorithm, data-structure, format-tool pages. Prefer updating an
   existing page over creating a near-duplicate.
5. Create `wiki/exercises/sheet-NN.md` for the exercise part, linking to the concepts used.
6. Update `wiki/overview.md`, `wiki/exam-prep.md`, and `wiki/index.md`.
7. Append a `log.md` entry.
8. Report: files created, files updated, contradictions found, open questions.

Default to one source at a time. Batch only if the user explicitly asks.

### Query
1. Read `index.md`, then the relevant pages (not the PDFs, unless the wiki is thin on the topic —
   then go to `raw/` and say that you did).
2. Answer with citations to wiki pages and to `raw/` locations.
3. If the answer is reusable (a comparison, a derivation, a synthesis across units), ask whether
   to file it in `wiki/queries/` as `YYYY-MM-DD-slug.md`, then link it from `index.md` and from
   the concept pages it touches.

### Lint
Trigger: *"lint the wiki"*. Check and report, grouped by severity:
- contradictions between pages
- orphan pages / broken wikilinks
- concepts referenced in prose but having no page
- `status: stub` pages that have been stale for several ingests
- algorithm pages missing pseudocode, structural pages missing a Mermaid diagram
- missing complexity tables, uncited claims
- coverage gaps vs. `overview.md`'s unit list

Propose fixes; apply only the ones the user approves.

## 7. index.md and log.md

`index.md` — grouped by the directory names in §1. One line per page:

```markdown
### Concepts
- [[suffix-array]] — lexicographically sorted suffix positions; O(n) space index for exact matching. *(units 3–4, solid)*
```

`log.md` — append-only, newest at the bottom. Entry headers use a fixed, greppable prefix so
`grep "^## \[" wiki/log.md | tail -5` works:

```markdown
## [2026-10-15] ingest | gik_3_4.pdf — Suffix arrays, BWT, FM-index
created: [[suffix-array]], [[bwt]], [[fm-index]], [[sheet-02]]
updated: [[index]], [[overview]], [[exam-prep]], [[exact-matching]]
notes: unit 4 exercise 3 contradicts the slide's complexity claim — flagged on [[fm-index]].
```

Entry kinds: `ingest`, `query`, `lint`, `refactor`.

## 8. Writing style

- Dense and precise. No filler, no "in this section we will". Write for a reader revising
  three days before the exam.
- Define every symbol used in a formula.
- Prefer a worked example over prose explanation whenever a worked example is possible.
- Use `> [!warning]` callouts for exam traps, `> [!note] Inference` for your own reasoning,
  `> [!question]` for unresolved points.
- Never invent content that is not in the sources. If you add standard background knowledge not
  present in the lecture, mark it `> [!note] Outside source` and say where it came from.
- Keep pages focused. If a page passes ~400 lines, split it and link the parts.
