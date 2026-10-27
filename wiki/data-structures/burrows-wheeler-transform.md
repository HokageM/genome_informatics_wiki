---
title: Burrows-Wheeler Transform
type: data-structure
units: [6]
sources: [gik_6]
tags: [bwt, fm-index, suffix-array]
status: solid
updated: 2026-10-15
---

# Burrows-Wheeler Transform

## Definition

The Burrows-Wheeler transform `bwt` of `S` is the string whose `i`-th character is the symbol
immediately left of suffix `S[SUF[i]..]`; for the suffix starting at `0`, the left character is
undefined or the sentinel (gik_6, p. 9).

## Why it matters

BWT is a reversible permutation that clusters similar characters and, with the LF mapping, becomes
a full-text index: the [[fm-index]] (gik_6, pp. 9-13).

## Construction / Mechanism

```mermaid
graph LR
  SA[Suffix array] --> BWT[preceding characters]
  BWT --> Rank[rank queries]
  Rank --> LF[LF mapping]
  LF --> Reconstruct[reconstruct S]
```

The LF mapping sends a row in the last column to the row with the same character occurrence in the
first column: `LF(i) = count[bwt[i]] + R_bwt(bwt[i], i) - 1` (gik_6, p. 12).

## Pseudocode

```text
ALGORITHM BuildBWT(S[0..n], SUF[0..n])
  for i = 0 to n:
    if SUF[i] = 0:
      bwt[i] <- sentinel
    else:
      bwt[i] <- S[SUF[i] - 1]
  return bwt

ALGORITHM ReconstructFromBWT(bwt, LF, startRow)
  i <- startRow
  for j = length(bwt) - 1 down to 0:
    S[j] <- bwt[i]
    i <- LF(i)
  return S
```

## Complexity

| Operation | Time | Space |
|---|---:|---:|
| Build from suffix array | `O(n)` | `O(n)` |
| Compute all rank values naively | `O(n + |A|)` | `O(n + |A|)` |
| One LF step with rank table | `O(1)` | table dependent |
| Reconstruct text | `O(n)` | `O(n)` |

## Worked example

For `S=banana$` and `SUF=[6,5,3,1,0,4,2]`, the preceding characters are `a,n,n,b,$,a,a`, so
`bwt=annb$aa`. LF follows equal occurrence numbers between the last and first columns; starting at
the `$` row reconstructs the text in reverse order (gik_6, pp. 10-12).

## Pitfalls & exam traps

> [!warning]
> - The last column is not arbitrary: it is tied to suffix-array order.
> - LF uses occurrence rank among equal characters, not the raw row number.
> - Reconstruction proceeds backwards; mixing direction reverses the string.

## Related

- [[suffix-array]] — BWT is computed from `SUF`.
- [[fm-index]] — stores sampled suffix-array data and rank support on BWT.
- [[fm-index-backward-search]] — uses BWT to update pattern intervals right-to-left.

## Sources

- gik_6, pp. 9-12
