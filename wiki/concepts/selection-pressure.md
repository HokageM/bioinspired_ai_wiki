---
title: Selection pressure
type: concept
sources: [L11]
tags: [evolution, optimisation]
updated: 2026-09-21
---

# Selection pressure

How strongly an algorithm prefers good individuals over bad ones. Named in
[[L11-evolutionary-computing]] as a marginal annotation to the linear-ranking
parameter ? **`s` = selection pressure** ? and it is the quantity the entire
lecture is implicitly about.

## One dial, four implementations

| Method | Dial | No pressure | Maximum pressure |
|---|---|---|---|
| [[ranking-selection]] | `s ? [1, 2]` | `s = 1` (uniform) | `s = 2` (worst never selected) |
| [[tournament-selection]] | `k` | `k = 1` (random) | `k = ?` (best always wins) |
| [[fitness-proportional-selection]] | **none** | ? | set by the data, not the engineer |
| [[survivor-selection]] | elitism `?`, age vs fitness | age-based | fitness-based, high `?` |

> [!note] FPS's real defect, restated
> It is the only method with **no dial**. Its pressure is whatever the
> distribution of fitness values happens to imply ? high when one individual
> dominates, near zero when values are similar, and changed by adding a constant.
> Ranking and tournament each expose a single parameter and make pressure a
> **design decision** rather than an accident.

## The trade-off it controls

```
low pressure                                        high pressure
  |                                                       |
  EXPLORATION                                    EXPLOITATION
  many regions searched                     one region refined
  slow convergence                          fast convergence
  may never converge                        premature convergence
  (drift)                                   (local optimum)
```

Both ends fail, differently. The lecture's practical advice is to watch **best
and average fitness together** ([[fitness-function]]): the gap between them *is*
the diversity of the population, and it is the readable symptom of where on this
line the run is sitting.

> [!note] The module's fifth architectural axis
> | Lecture | Axis |
> |---|---|
> | L05 | learned ? specified |
> | L07 | fast ? contextual |
> | L08 | modelless ? modelled |
> | L09 | stimulus-driven ? goal-driven |
> | **L11** | **exploration ? exploitation** |
>
> This one is different in kind: the first four describe where an *architecture*
> sits, and this describes how a *search* is run. It is also the axis that
> reinforcement learning is organised around ? and RL, named-only since L03,
> still never appears. The module now has the exploration/exploitation trade-off
> without the paradigm that is usually taught alongside it.

## Where else pressure appears without the name

- [[recombination]] with `p_c ? [0.5, 1.0]` ? more crossover, more exploration.
- [[mutation]]'s `p_m` ? the same, at smaller scale.
- The *"do not constrain the search space too much"* rule of thumb: an
  over-specified [[fitness-function]] is selection pressure applied by the
  engineer rather than by the algorithm.

## See also

- [[premature-convergence]] ? [[fitness-landscape]] ? [[parent-selection]]
