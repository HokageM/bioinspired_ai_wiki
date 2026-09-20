---
title: CaRL
type: system
sources: [L13]
tags: [continual-learning, methods]
updated: 2026-09-21
status: stub-by-source
---

# CaRL

> e.g.: **CaRL**

Named once, as the example of the
[[continual-learning-strategies|rehearsal/replay]] family.
`status: stub-by-source` ? the acronym is not expanded and no mechanism is given.

> [!warning] Probably *iCaRL*, but the notes say "CaRL"
> *iCaRL ? Incremental Classifier and Representation Learning* is the standard
> rehearsal-based continual learning method and the near-certain referent
> [external]. The notes write **CaRL**, so this page records what the source says
> and flags the inference rather than silently correcting it.

What the family implies: store a subset of past samples (an *exemplar set*) and
include them in every subsequent training batch, so the old task stays visible to
the optimiser. Cost: **memory and compute grow with the number of tasks**.

## See also

- [[continual-learning-strategies]] ? [[memory-replay]] ?
  [[growing-dual-memory]]
