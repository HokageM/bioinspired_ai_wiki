---
title: Candidate representation
type: concept
sources: [L11]
tags: [evolution, representation, optimisation]
updated: 2026-09-21
---

# Representation of candidate solutions

Two requirements, stated as constraints on the search space:

> - **Need to cover all possible solutions**
> - **Should only allow valid solutions**

The first is *completeness* ? if the optimum cannot be encoded, no amount of
search will find it. The second is *closure* ? if invalid genomes exist, every
operator must either avoid producing them or the algorithm must waste evaluations
on them.

They pull against each other: a tight encoding that admits only valid solutions
usually also excludes some valid ones, and a permissive encoding admits garbage.

## The bit-string problem

> **Bit-string can decode integers or real numbers.**
> **Problem: e.g. mutation changes value significantly.**
> **Better: direct representation of numbers.**

Flipping the top bit of `10000000` gives `00000000` ? a change of 128 from a
single-bit [[mutation|bitwise mutation]], while flipping the bottom bit changes
the value by 1. The mutation operator is *uniform over bits* and wildly
non-uniform over **values**, so the step size is an accident of position.

> [!note] Locality: small genotype change should mean small phenotype change
> The bit-string failure is a failure of **locality** in the
> [[genotype-and-phenotype|genotype-phenotype map]], and it is the reason
> [[evolutionary-algorithm|Evolution Strategies]] use real-valued vectors: adding
> a small Gaussian to a real number is a small change *by construction*.
>
> The lecture reaches the right conclusion ? *"better: direct representation of
> numbers"* ? without naming the principle. It is the same idea as Gray coding
> [external], which the source does not mention.

## Representation determines the operators

Not the other way round. From [[evolutionary-algorithm]]:

> **Variation operators depend on candidate representation.**

| Representation | Recombination | Mutation |
|---|---|---|
| bit string | n-point, uniform | flip a bit with `p_m` |
| real vector | arithmetic | add noise / random resetting |
| tree | swap subtrees | change a node |
| **neural network** | **hard** ? see below | change weights, add/delete nodes |

## Direct and indirect mapping

> **Direct:** gene directly represents feature in phenotype space.
> **Indirect:** generative encoding, e.g. grammar.

See [[genotype-and-phenotype]]. The indirect case gets three words and is the
most powerful option on the page.

## See also

- [[genetic-neural-encoding]] ? [[mutation]] ? [[recombination]] ?
  [[genotype-and-phenotype]]
