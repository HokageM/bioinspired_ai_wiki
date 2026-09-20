---
title: Parent selection
type: concept
sources: [L11]
tags: [evolution, optimisation]
updated: 2026-09-21
---

# Parent selection

> - **Select individuals to create offspring**
> - **Selection happens at population level**
> - **Selection is often probabilistic:**
>   - individuals with high fitness more likely to become parents
>   - **"weak" individuals might also become parents to avoid local optima**
>   - `? p? = 1`
> - Parent selection supports process of evolving better solutions over time

## Why probabilistic rather than "take the best"

The second bullet is the whole argument. Deterministically taking the top
individuals is a greedy hill-climb: it converges fast and it converges to
whatever basin the initial population happened to land in.

Keeping weak individuals in play is **exploration**, paid for in convergence
speed. The bound `? p? = 1` says only that this is a probability distribution
over the population ? the *shape* of that distribution is
[[selection-pressure]], and every method below is a different shape.

## The four methods, and what each needs to know

| Method | Requires | Cost |
|---|---|---|
| [[fitness-proportional-selection]] | every fitness value, globally | premature convergence; scale-sensitive |
| [[ranking-selection]] | a global **sort** | slower convergence |
| [[roulette-wheel-selection]] | a distribution, globally | high sampling variance; expensive when distributed |
| [[tournament-selection]] | **only pairwise comparison** | selection pressure set by `k` |

> [!note] The progression is a steady removal of global knowledge
> FPS needs the actual numbers and their sum. Ranking needs only the order, so
> the fitness *scale* stops mattering. Tournament needs neither ? just the
> ability to say *"this one is better than that one"* within a random group of
> `k`.
>
> The lecture states the endpoint explicitly ? *"tournament selection only
> requires order between individuals (relative fitness) and no global knowledge;
> previous methods: global"* ? and does not present it as a progression. It is
> one, and it is why tournament selection is the default in practice: it
> parallelises, it is cheap, and its pressure is tunable with one integer.

## Parent selection versus survivor selection

Both select on quality; they differ in *when* and *how hard*:

| | Parent selection | [[survivor-selection]] |
|---|---|---|
| Timing | before variation | after variation |
| Usually | **stochastic** | **deterministic** |
| Purpose | decide who reproduces | decide who persists |

> [!note] One stochastic stage and one deterministic stage
> Soft at the front, hard at the back. If both were deterministic the population
> would collapse to clones; if both were stochastic, good solutions could be lost
> by chance. Elitism at the survivor stage guarantees the best is never lost,
> while probabilistic parent selection keeps the gene pool open. The division of
> labour is not remarked on in the source.

## See also

- [[selection-pressure]] ? [[premature-convergence]] ? [[evolutionary-algorithm]]
