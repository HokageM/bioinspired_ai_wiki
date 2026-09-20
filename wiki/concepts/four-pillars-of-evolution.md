---
title: The four pillars of evolution
type: concept
sources: [L11]
tags: [evolution, biology]
updated: 2026-09-21
---

# The four pillars of evolution

> All species derive from common ancestors.
>
> - **Population** ? group of several individuals
> - **Diversity** ? individuals have different characteristics
> - **Heredity** ? characteristics are transmitted over generations
> - **Selection** ? individuals produce more offspring than the environment can
>   support. *Better at food gathering = better at surviving = make more
>   offspring.*

## Why exactly four

These are the **sufficient conditions** for evolution. Remove any one and the
process stops:

| Remove | What happens |
|---|---|
| Population | one individual, nothing to select between |
| Diversity | every individual identical, selection has no purchase |
| Heredity | good traits die with their bearer; no accumulation |
| Selection | drift only; no direction |

Nothing about DNA, proteins or biology appears in that list. Evolution is
**substrate-neutral** ? it will run on anything that has the four properties, and
that is exactly why it can be run on candidate solutions in a computer.

> [!note] The pillars are the algorithm
> | Pillar | Component of an [[evolutionary-algorithm]] |
> |---|---|
> | Population | the population array |
> | Diversity | [[mutation]] and [[recombination]] |
> | Heredity | the genotype is copied to offspring |
> | Selection | [[parent-selection]] and [[survivor-selection]] |
>
> The lecture presents the biology on page 1 and the algorithm on page 3 with
> this correspondence left implicit. It is the entire justification for the
> method and deserves to be stated.

## The circularity

*"Better at food gathering = better at surviving = make more offspring"* is
written as a chain of equalities, which makes fitness both the cause and the
measure of reproductive success. As biology that is circular.

It is **not** circular in an algorithm, because the engineer **stipulates** the
fitness function in advance and it is independent of how many offspring anything
has. That difference is the single most important thing separating the metaphor
from the method ? see [[fitness-function]].

## See also

- [[genotype-and-phenotype]] ? [[dna-and-heredity]] ? [[evolutionary-algorithm]]
