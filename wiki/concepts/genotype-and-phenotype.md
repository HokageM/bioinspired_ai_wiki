---
title: Genotype and phenotype
type: concept
sources: [L11]
tags: [evolution, biology, representation]
updated: 2026-09-21
---

# Genotype and phenotype

> **Phenotype:** manifestation of the organism (appearance, etc.)
> - selection operates on the phenotype
> - affected by environment, development and learning
>
> **Genotype:** the genetic material of that organism
> - transmitted during reproduction
> - affected by mutations
> - **selection does not operate directly on it**
>
> **Genetics:** structure and operation of genes.
> **Functional genomics:** role of genes in the organism.
>
> **Complex, indirect mapping between genotype and phenotype.**

## The asymmetry is the whole design

| | Genotype | Phenotype |
|---|---|---|
| What it is | the code | the thing the code builds |
| [[mutation]] and [[recombination]] act on | **this** | ? |
| [[fitness-function]] and selection act on | ? | **this** |

Variation operates at one end, evaluation at the other, and they are joined by a
**decoding step** that the algorithm designer chooses. Everything hard in
evolutionary computing lives in that step.

```
genotype  --decode-->  phenotype  --evaluate-->  fitness
    ^                                               |
    +-------- mutate / recombine <-- select --------+
```

## Direct and indirect mapping

> **Direct:** gene directly represents feature in phenotype space.
> **Indirect:** generative encoding, e.g. grammar.

**Direct** is the default: bit 7 of the string *is* the seventh weight. Simple,
debuggable, and the genome grows linearly with the solution.

**Indirect** encodes a *procedure that builds* the phenotype, so a short genome
can specify a large, repetitive structure ? which is what DNA actually does. It
also makes mutations **structured**: one changed symbol alters a rule, and the
rule is applied everywhere.

> [!note] The most interesting idea on the page, given three words
> *"Generative encoding, e.g. grammar"* is the entire treatment. It is the route
> to evolving large networks from small genomes, and the source does not develop
> it, give an example, or name a method.

## Why the distinction matters for learning

The phenotype is *"affected by environment, development and **learning**"* ? so
an individual can change within its lifetime without changing its genotype.

> [!note] The module has the ingredients for the Baldwin effect and does not use them
> L11 notes that neuro-genetic hybrids *"can be combined with learning"*
> ([[neuroevolution]]). A learned improvement is phenotypic and is not inherited,
> but it changes the **fitness** of the genotype that made it learnable, which
> lets learning steer evolution without Lamarckian inheritance. [external] That
> is the Baldwin effect; the lecture assembles every piece and does not name it.

## See also

- [[dna-and-heredity]] ? [[candidate-representation]] ?
  [[four-pillars-of-evolution]]
