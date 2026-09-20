---
title: Neuro-genetic hybrid architectures
type: system
sources: [L11]
tags: [evolution, neural-networks, architecture]
updated: 2026-09-21
---

# Neuro-genetic hybrid architectures

The union the lecture set out to build: *"integrate evolutionary algorithms and
neural networks."*

> **Pros:**
> - **Can also evolve weights, topology and hyper-parameters at the same time**
> - **Can be combined with learning**
>
> **Cons:**
> - **Evolved NN often complex**
> - **Competing Conventions Problem (same function in different way)**
> - **Search space usually complex and too many local minima**

## Why it answers the opening question

[[L11-evolutionary-computing]] opens by asking *"how to select necessary
topology, weights and efficient learning parameters?"* ? three things
[[backpropagation]] cannot touch, because two of them are **discrete** and one
governs the gradient procedure itself.

An [[evolutionary-algorithm]] needs only a [[fitness-function]], so all three
become searchable in the same run. That is the entire argument, and it is a good
one.

```
genome = (topology, weights, hyperparameters)

def fitness(genome):
    net = decode(genome)
    # optionally: net = train(net)        <- "can be combined with learning"
    return performance(net, task)
```

## "Can be combined with learning"

Five words containing the most interesting idea here. Evolution sets the
**starting point and the structure**; gradient learning refines the weights
within a lifetime. Each does what it is good at ? evolution searches discrete,
non-differentiable structure; backpropagation exploits gradients where they
exist.

> [!note] Every ingredient of the Baldwin effect, unnamed
> Learned improvements are phenotypic and are **not** inherited
> ([[genotype-and-phenotype]]), but they change the measured fitness of the
> genotype that made them **learnable**. So evolution is steered towards genomes
> that learn well, without any Lamarckian inheritance. [external] The lecture
> assembles the pieces across two pages and never names the effect, which is
> arguably the most important idea in the intersection of learning and evolution.

## The cons, ranked by seriousness

**Competing conventions** is the fundamental one ? see
[[competing-conventions-problem]]. It makes [[recombination]] unreliable, and
without reliable recombination an EA degenerates into
[[mutation|"a parallel gradient search"]], by the lecture's own words.

**Too many local minima** is a property of the search space that
[[genetic-neural-encoding]] creates ? and competing conventions is one of its
causes, since every permutation of a good solution is its own optimum with
valleys in between.

**Evolved NN often complex** is the parsimony problem. Nothing in the fitness
function rewards simplicity, so networks accumulate structure that does no harm.
[[mutation]]'s rule `Prob(delete) > Prob(add)` is a partial remedy applied at the
operator level rather than in the objective.

> [!note] Structure search, twice in two lectures, by opposite means
> | | [[gwr-network|GWR]] (L10) | Neuroevolution (L11) |
> |---|---|---|
> | When | **during** learning, online | **across** generations |
> | Driven by | how well the input is covered | fitness of the whole network |
> | Candidates | one network | a population |
> | Signal needed | a distance | a scalar score |
> | Cost | cheap, incremental | expensive, many evaluations |
>
> Consecutive lectures, the same problem ? *how big should the network be?* ?
> and no cross-reference in either direction.

## See also

- [[competing-conventions-problem]] ? [[genetic-neural-encoding]] ?
  [[evolutionary-algorithm]] ? [[gwr-network]]
