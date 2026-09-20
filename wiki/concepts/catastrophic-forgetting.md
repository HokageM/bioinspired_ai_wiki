---
title: Catastrophic forgetting
type: concept
sources: [L13]
tags: [continual-learning, memory, learning, representation]
updated: 2026-09-21
---

# Catastrophic forgetting

> - **Training a model with new information interferes with previously learned
>   knowledge**
> - **Performance degradation (interference) all the way to a complete
>   overwriting (forgetting) of old knowledge with new one**
> - **Catastrophic forgetting affects all connectionist models**

Two words for one spectrum: **interference** is partial, **forgetting** is total.

## Why it happens

The lecture asserts it rather than deriving it, but the derivation is short and
the module has all the pieces:

1. A network stores every task in the **same** weights
   ([[local-vs-distributed-representation|distributed representation]]).
2. [[backpropagation]] moves those weights to reduce error on the **current**
   batch only. Nothing in the gradient refers to a task it is not currently
   seeing.
3. The old task's error is therefore not merely un-preserved ? it is **invisible**.

There is no forgetting mechanism to find. Forgetting is what optimising one
objective does to an unrelated one that shares parameters.

> [!note] The distributed-representation trade-off, third appearance, third sign
> | | Verdict on distributed coding |
> |---|---|---|
> | **L02** | **advantage** ? robust, generalises, degrades gracefully |
> | **L12** | **cost** ? [[local-vs-distributed-representation|illegible]], hard to modify |
> | **L13** | **cost** ? **it is the cause of catastrophic forgetting** |
>
> All three are consequences of one fact: **a concept has no address.** You cannot
> damage it selectively (L02's benefit), you cannot read it (L12's cost), and you
> cannot protect it while changing everything else (L13's cost). The module never
> states the shared root. This wiki does.

> [!warning] "Affects all connectionist models" is too strong, by the lecture's own next page
> A network with **disjoint** parameters per task cannot forget ? nothing shared,
> nothing overwritten. That is exactly what
> [[continual-learning-strategies|dynamic architectures]] and
> [[progressive-neural-network]] do, two pages later, and they are connectionist.
>
> The accurate claim is: *catastrophic forgetting affects any model that reuses
> parameters across tasks and optimises them on the current task alone.* The
> strength of the original statement is what makes the strategies look necessary.

## The cost of not forgetting

Protection is not free, and every strategy pays in a different currency:
[[continual-learning-strategies]].

## See also

- [[continual-learning]] ? [[stability-plasticity-dilemma]] ?
  [[continual-learning-metrics]] ? [[local-vs-distributed-representation]]
