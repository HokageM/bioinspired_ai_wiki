---
title: Premature convergence
type: concept
sources: [L11]
tags: [evolution, optimisation]
updated: 2026-09-21
---

# Premature convergence

The population collapses onto one region of the [[fitness-landscape]] before the
search has explored enough of it, and thereafter cannot leave. Named in
[[L11-evolutionary-computing]] as the first problem of
[[fitness-proportional-selection]].

## The mechanism

```
gen 0:   ..x..x....x...x..x..      diverse
gen 5:   ....xx.xx.x..........      one region winning
gen 12:  .....xxxxxx..........      all individuals nearly identical
```

Once individuals are nearly identical, [[recombination]] becomes a no-op ?
combining two copies of the same genome returns that genome ? and only
[[mutation]] generates novelty, one small step at a time. The population has
become a single hill-climber with `?` times the cost.

> [!note] Diversity is the resource the algorithm consumes
> It is present at initialisation and is spent by selection. Nothing restores it
> except mutation. That framing makes every operator choice legible: selection
> spends diversity to buy progress, and the run ends when diversity is gone ?
> which is why *"diversity of population too small"* is listed as a legitimate
> **termination condition**.

## Causes, as given across the lecture

| Cause | Page |
|---|---|
| One individual with far higher fitness dominating the distribution | [[fitness-proportional-selection]] |
| Selection pressure too high | [[selection-pressure]] |
| Fitness-based [[survivor-selection]] keeping a local optimum forever | [[survivor-selection]] |
| An over-detailed fitness function constraining the search | [[fitness-function]] |

## Remedies, also given, also scattered

- **Let weak individuals reproduce.** *"'Weak' individuals might also become
  parents to avoid local optima"* ? [[parent-selection]].
- **Use rank, not value.** [[ranking-selection]] removes the runaway-outlier
  cause outright.
- **Lower the dial.** Smaller `k`, smaller `s`.
- **Age-based survivor selection** ? turn the population over.
- **Do not over-constrain.** *"Let evolution do its job."*

The lecture never collects these on one page, and they are the same answer given
five times.

## The opposite failure

Too little pressure is also fatal and less often noticed: the population drifts,
best fitness never improves, and the run terminates on *"no fitness
improvement"* having searched randomly. The lecture names this as FPS's second
problem ? *"nearly equal fitness values result in no selection pressure"* ? and
does not give it a name.

## See also

- [[selection-pressure]] ? [[fitness-landscape]] ? [[fitness-function]]
