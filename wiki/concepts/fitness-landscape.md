---
title: Fitness landscape
type: concept
sources: [L11]
tags: [evolution, optimisation]
updated: 2026-09-21
---

# Fitness landscape

> - Usually of very **high dimension**
> - **Local optimum:** solution better than neighbouring solutions
> - **Global optimum:** best solution in landscape
> - **Uni-modal problem:** only one local optimum
> - **Multi-modal problem:** several local optima

The genotype space, with fitness as height. *Neighbouring* means *reachable by
one [[mutation]]*, so **the landscape is defined by the operators**, not by the
problem alone: change the mutation operator and the same problem gets a different
landscape, with different local optima.

## Why a population helps

A single hill-climber stops at the first local optimum it reaches. A population
samples many regions at once, and [[recombination]] can **jump** between them ?
which no local method can do.

```
single point:     x ??? local optimum ??? stuck
population:       x  x     x   x          several basins occupied
recombination:    combine parts of two basins ??? a point in neither
```

That last line is also why the lecture calls recombination *"often a destructive
jump in fitness landscape"*: the combined point is usually **worse** than both
parents. It is a high-variance move, which is what exploration costs.

## The connection to L08

> [!note] The module's second encounter with local optima
> [[local-minima-problem]] (L08) described a robot trapped in a
> [[potential-field-navigation|potential field]] ? stuck in a local minimum of a
> hand-designed function, in a 2-D space you can draw.
>
> L11's landscape is the same mathematical object in *"very high dimension"*, for
> a function nobody designed. The solution differs accordingly: L08 added
> noise or a random walk to one agent; L11 keeps a **population** and mixes it.
>
> The lectures are three apart and neither refers to the other, though it is the
> same problem with the same name.

## Uni-modal and multi-modal

If a problem were truly uni-modal, an EA would be the wrong tool ? gradient
descent or hill climbing would be faster and would come with guarantees. EAs earn
their cost only on **multi-modal** landscapes, and the lecture's own cons list
agrees: *"no guarantee in optimal solution; long runtimes compared to search
algorithms"* (see [[genetic-inverse-kinematics]]).

## See also

- [[fitness-function]] ? [[premature-convergence]] ? [[selection-pressure]] ?
  [[local-minima-problem]]
