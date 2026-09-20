---
title: Roulette wheel selection
type: system
sources: [L11]
tags: [evolution, optimisation, methods]
updated: 2026-09-21
---

# Roulette wheel (RW)

> **FPS and ranking selection define probability distributions for selecting
> individuals.**
> **Sample from distributions: Roulette Wheel (RW).**

An important distinction the lecture makes clearly:
[[fitness-proportional-selection|FPS]] and [[ranking-selection]] produce a
**distribution**; the roulette wheel is a **sampler**. They answer different
questions and are often conflated.

```
def roulette_wheel(population, p):
    cum = cumulative_sum(p)               # the wheel: one arc per individual
    parents = []
    for _ in range(mu):
        r = random()                      # one spin per parent
        parents.append(population[first_index_where(cum >= r)])
    return parents
```

The marginal diagram shows the wheel divided into arcs A, B, C with a single
pointer.

## Sampling quality in practice

> - **High variance from theoretical distribution**
> - **Expensive to calculate in distributed systems, if number of parents `?` is
>   large**

**Variance.** `?` independent spins do not deliver `? ? p_i` copies of individual
`i`; they deliver a binomial sample around it. An individual with `p = 0.1` in a
population of 10 is *expected* to be chosen once and will quite often be chosen
zero times. The good solution is lost to a coin flip, not to selection.

> [!note] The standard fix is not given
> [external] Stochastic universal sampling uses **one** random number and `?`
> equally-spaced pointers on the same wheel, which guarantees each individual
> gets either `floor(??p_i)` or `ceil(??p_i)` copies ? same expectation, almost
> no variance, one spin. The lecture names the defect and not the remedy.

**Distributed cost.** The cumulative sum requires **every** fitness value in one
place, so the whole population must be gathered before a single parent can be
chosen. That is a synchronisation barrier in an algorithm that is otherwise
embarrassingly parallel ? and it is the specific reason
[[tournament-selection]] wins on large or distributed populations, since a
tournament needs only `k` individuals, chosen anywhere.

## See also

- [[fitness-proportional-selection]] ? [[tournament-selection]] ?
  [[parent-selection]]
