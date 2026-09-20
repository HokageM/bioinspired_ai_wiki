---
title: Tournament selection
type: system
sources: [L11]
tags: [evolution, optimisation]
updated: 2026-09-21
---

# Tournament selection

> **Tournament selection only requires order between individuals (relative
> fitness) and no global knowledge.**
> **Previous methods: global.**

## The algorithm

> `k` = tournament size (**larger size = larger selection pressure**)
>
> - randomly select `k` individuals into a group `G` of contestants
> - individual `i` with `f(i) = max_{i?G} f(i)` wins the tournament
> - add `i` to parent set
> - repeat until `?` parents are selected

```
def tournament_selection(population, mu, k):
    parents = []
    while len(parents) < mu:
        G = random_sample(population, k)      # no global knowledge needed
        parents.append(argmax(G, key=f))      # only comparisons within G
    return parents
```

## Why `k` is selection pressure

An individual wins only if it is the best of its group. The probability that the
very worst individual ever wins is the probability that it is drawn *alone*,
which falls off as `k` grows. At `k = 1` selection is uniform random ? no pressure
at all. At `k = ?` the best individual wins every tournament and nothing else
ever reproduces.

| `k` | Behaviour |
|---|---|
| 1 | random drift |
| 2 | gentle pressure, the common default [external] |
| large | strong pressure, fast convergence, diversity loss |

One integer, tuning the whole [[selection-pressure|exploration?exploitation]]
trade-off ? compare [[ranking-selection]]'s `s ? [1, 2]`, which does the same job
with a real number and a global sort.

## Why "no global knowledge" matters

No sum, no sort, no normalisation, no gathering of the population. A tournament
can be run on any `k` individuals wherever they happen to be, which makes the
method:

- **parallel** ? many tournaments at once, no synchronisation barrier;
- **cheap** ? `O(k)` per parent instead of `O(?)` or `O(? log ?)`;
- **scale-free** ? immune to
  [[fitness-proportional-selection|transposition sensitivity]] for the same
  reason ranking is, since only comparisons are used.

It fixes everything FPS got wrong *and* everything
[[roulette-wheel-selection|the roulette wheel]] got wrong, with less machinery
than either.

> [!note] Argmax again ? instance nine
> A tournament is a [[winner-take-all]] competition over a random subset. The
> module's most reused primitive, appearing now in a lecture that shares no other
> vocabulary with the nine before it: [[self-organising-map|BMU]],
> [[fusion-strategies|max fusion]], [[behaviour-coordination|action selection]],
> [[subsumption-architecture|subsumption]], [[saliency-model|saliency]],
> [[auditory-scene-analysis|auditory objects]],
> [[contrastive-language-image-pretraining|CLIP]], and here.
>
> The difference: every earlier instance is argmax over a **fixed** set. This one
> is argmax over a **random sample**, and randomising the competitor set is
> exactly what converts a greedy rule into a tunable one.

## See also

- [[parent-selection]] ? [[selection-pressure]] ? [[ranking-selection]]
