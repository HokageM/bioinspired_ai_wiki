---
title: Ranking selection
type: system
sources: [L11]
tags: [evolution, optimisation]
updated: 2026-09-21
---

# Ranking selection

> - **Inspired by problems of fitness proportional selection**
> - **Rank individuals by fitness**
> - **Assign selection probabilities based on rank**
> - **Problems: can lead to slower convergence**
>
> **Linear Rank (LR):**
> `P_LR(i) = (2 ? s)/? + 2i(s ? 1)/(?(? ? 1))`, with `s ? [1, 2]`
> annotated: **`s` = selection pressure**

```
def linear_rank(population, s):          # s in [1, 2]
    ranked = sort(population, key=f)     # worst first, rank i = 0 .. mu-1
    mu = len(ranked)
    return [ (2 - s)/mu + 2*i*(s - 1)/(mu*(mu - 1)) for i in range(mu) ]
```

## Why this fixes FPS

Ranking discards the fitness **values** and keeps only the **order**, so:

- adding a constant to every fitness changes nothing ?
  [[fitness-proportional-selection|transposition sensitivity]] gone;
- one outlier cannot dominate the distribution, because it gets rank `??1`
  whether it is twice as good or a thousand times as good;
- nearly-equal fitnesses still produce full pressure, because ranks are spaced
  evenly by construction.

All three FPS problems, removed by a sort.

## Reading the formula

At the two ends of the allowed range:

| `s` | Distribution | Behaviour |
|---|---|---|
| `s = 1` | `P = 1/?` for all `i` | **uniform** ? no selection pressure at all, pure drift |
| `s = 2` | `P = 2i/(?(??1))`, worst gets 0 | **maximum linear pressure** ? the worst individual never reproduces |

So `s` is a single dial from *no selection* to *maximum linear selection*, and
the probabilities sum to 1 for every value in between. That is what the marginal
note **"s = selection pressure"** means, and it is the cleanest parameterisation
in the lecture ? see [[selection-pressure]].

## The cost

> **Can lead to slower convergence.**

Correct, and it is the direct consequence of the fix: by refusing to let a very
good individual take a proportionally large share, ranking also refuses to
exploit it. Ranking buys robustness with generations. And it requires a **global
sort**, so it still needs global knowledge ? which is what
[[tournament-selection]] then removes.

## See also

- [[fitness-proportional-selection]] ? [[selection-pressure]] ?
  [[tournament-selection]]
