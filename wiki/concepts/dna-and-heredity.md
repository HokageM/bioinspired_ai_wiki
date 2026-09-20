---
title: DNA, cell replication and heredity
type: concept
sources: [L11]
tags: [evolution, biology]
updated: 2026-09-21
---

# DNA, cell replication and heredity

## DNA

> Humans have **23 pairs** of DNA molecules (chromosomes).
> Nucleotides `A, C, G, T`, pairing `A?T` and `C?G`.
> **A gene is a sequence of several nucleotides that produce a certain protein.**
> **Complex, indirect mapping between genotype and phenotype.**

## Cell replication

> **Mitosis:** replication during growth / maintenance of the organism.
> **Meiosis:** during production of gametes (egg / sperm cells).

Meiosis is the interesting one, and the source marks it as *a special form of
cell splitting*:

> - **Produces gametes:** sperm / egg cell which contains only one single
>   chromosome complement of chromosomes
> - **During meiosis, pairs of chromosomes undergo crossing-over**
> - Occasionally some of the genetic material changes very slightly
>   (replication error): **mutation**

The marginal diagrams record the difference: mitosis *"division into two
identical daughter cells"*; meiosis *"division produces haploid sex cells"*, which
*"fuse into a diploid cell during fecundation"*.

## The two sources of variation, and their biological originals

| Biology | Algorithm |
|---|---|
| **crossing-over** during meiosis | [[recombination]] / crossover |
| **replication error** | [[mutation]] |

That is the entire mapping, and it explains why the two operators have the
properties they do:

- **Crossover exchanges contiguous segments**, because chromosomes physically
  break and rejoin at points along their length. Hence
  [[recombination|n-point crossover]] and its **positional bias** ? genes that
  sit close together tend to travel together. The bias is inherited from the
  physical mechanism, not chosen.
- **Mutation is small, local and rare**, because a replication error is a
  copying slip.

> [!note] Uniform crossover has no biological original
> [[recombination|Uniform crossover]] swaps each gene independently, which
> removes the positional bias ? and there is no chromosomal mechanism that does
> it. The lecture lists it beside n-point without noting that this one is pure
> engineering. See [[ann-brain-correspondence]] for the module's running problem
> of distinguishing what is copied from biology from what merely sounds like it.

## The 23 pairs and what they imply

**Diploid** ? two copies of each chromosome, so an organism carries alternative
alleles and can hide a recessive one. That is a **reservoir of diversity** that
survives selection invisibly.

Essentially all evolutionary algorithms, including every one in this lecture, are
**haploid** ? one genome per individual, no dominance, no hidden variation. The
notes give the biology and the algorithm and do not remark on the simplification.

## See also

- [[genotype-and-phenotype]] ? [[recombination]] ? [[mutation]] ?
  [[four-pillars-of-evolution]]
