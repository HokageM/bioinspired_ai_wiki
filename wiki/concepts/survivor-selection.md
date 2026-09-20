---
title: Survivor selection
type: concept
sources: [L11]
tags: [evolution, optimisation]
updated: 2026-09-21
---

# Survivor selection

> Similarly to parent selection, survivor selection **selects individuals based
> on quality**.
> **Role: reduce `?` parents and `?` offspring to `?` individuals that constitute
> the next generation.**
> **Selection often deterministic**, e.g. based on age and/or fitness.
> Synonyms: **environmental selection, replacement**.

## The three policies

> - **Age-based:** choose `?` best from **offspring only**
> - **Fitness-based:** choose `?` best from **parents and offspring**
> - **Elitism:** keep the `? < ?` best, replace the rest by offspring
> - Often fixed, e.g. **keep 10% elites, 40% mutated, 50% recombined**

```
def survivor_selection(parents, offspring, mu, mode, kappa=0):
    if mode == "age":      pool = offspring                 # (mu, lambda)
    if mode == "fitness":  pool = parents + offspring       # (mu + lambda)
    if mode == "elitism":
        elites = best(parents, kappa)
        return elites + best(offspring, mu - kappa)
    return best(pool, mu)
```

## Age-based versus fitness-based: the real trade-off

**Fitness-based** keeps the best individual forever. It can never get worse, and
it can never escape: a strong individual that sits on a local optimum stays in
the population indefinitely and keeps winning
[[parent-selection|parent selection]], so the whole population drifts towards it.
That is [[premature-convergence]] with a ratchet.

**Age-based** discards every parent regardless of quality. The best solution
found can be **lost**, and the population can move downhill ? which is exactly
what lets it cross a valley.

> [!note] This is simulated annealing's dilemma, without the vocabulary
> Accept-only-improvements gets stuck; accept-worse-sometimes escapes.
> [external] The EA resolves it structurally instead of by a temperature: keep a
> **few** elites so the best is never lost, and let the rest turn over so the
> population keeps moving. The stated 10 / 40 / 50 default is exactly that
> compromise, given as a recipe with no derivation.

## Where the pressure actually lives

[[parent-selection]] is *"often probabilistic"*; survivor selection is *"often
deterministic"*. So the hard cut happens **after** variation, on evidence: every
offspring has been evaluated before anything is discarded.

> [!note] Evaluate, then cull ? never the reverse
> A deterministic parent-selection stage would discard individuals on their
> *current* fitness, before they had a chance to produce offspring. Doing the
> deterministic step last means the algorithm never throws away a solution whose
> children it has not seen.

## See also

- [[parent-selection]] ? [[selection-pressure]] ? [[premature-convergence]] ?
  [[evolutionary-algorithm]]
