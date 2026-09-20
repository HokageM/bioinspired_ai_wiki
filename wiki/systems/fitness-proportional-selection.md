---
title: Fitness proportional selection (FPS)
type: system
sources: [L11]
tags: [evolution, optimisation]
updated: 2026-09-21
---

# Fitness proportional selection (FPS)

> **Absolute fitness of individual vs absolute fitness of population.**
> **Probability to select `i` from population of size `?`:**
>
> `Pr(i) = f_i / ?_j^? f_j`

```
def fps(population):
    total = sum(f(i) for i in population)
    return [f(i) / total for i in population]     # a distribution over individuals
```

Then sample it ? see [[roulette-wheel-selection]].

## The three problems

> - **Premature convergence**
> - **Nearly equal fitness values result in no selection pressure**
> - **Selection probabilities change for transposed fitness value**

Each is a distinct failure, and together they are why FPS is rarely used
unmodified.

**Premature convergence.** One individual with fitness far above the rest takes a
large share of the distribution immediately, and its descendants dominate within
a few generations. See [[premature-convergence]].

**No pressure when fitnesses are similar.** Late in a run everything is nearly
equally good, so every `p_i ? 1/?` and selection becomes a **uniform random
draw**. The algorithm keeps running and stops improving.

> [!note] FPS has exactly the wrong dynamics at both ends
> Too much pressure early, when you want exploration; too little late, when you
> want refinement. [[ranking-selection]] fixes both at once because rank spacing
> is constant regardless of how the raw values are distributed.

**Transposition sensitivity.** Add a constant to every fitness and the
probabilities change:

```
f = (1, 2, 3)        ->  p = (0.17, 0.33, 0.50)     ratio best:worst = 3.0
f = (101, 102, 103)  ->  p = (0.33, 0.33, 0.34)     ratio best:worst = 1.02
```

Same problem, same ordering, same differences ? and the selection pressure has
almost vanished. The algorithm's behaviour depends on an arbitrary **offset** of
the objective function, which is a property no optimiser should have. It also
means FPS requires **non-negative** fitness values, which the source does not
mention.

## See also

- [[ranking-selection]] ? [[roulette-wheel-selection]] ? [[parent-selection]]
