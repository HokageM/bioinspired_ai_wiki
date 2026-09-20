---
title: Evolved collision-free navigation
type: system
sources: [L11]
tags: [evolution, robotics, embodiment]
updated: 2026-09-21
---

# Evolved collision-free navigation

The lecture's worked example of a full EA configuration:

> **Mutation:** small value changes
> **Crossover:** 1-point crossover
> **Selection:** Roulette Wheel
>
> **Fitness** `? = V (1 ? ??V)(1 ? i)`
>
> - `V` ? rotation speed
> - `?V` ? difference of speeds of wheels
> - `i` ? activation of highest sensor

## Reading the fitness function

Three factors, **multiplied**, each normalised to roughly `[0, 1]`:

| Factor | Maximised when | Encourages |
|---|---|---|
| `V` | wheels turning fast | **move** |
| `1 ? ??V` | both wheels at the same speed | **go straight** |
| `1 ? i` | no sensor strongly activated | **stay away from obstacles** |

> [!note] Multiplication makes it a conjunction, not a compromise
> A weighted **sum** would let a robot spin fast in open space and score well on
> two terms. A **product** means any factor near zero drives the whole score to
> zero, so all three must hold at once. That is the difference between *"do these
> things on average"* and *"do all of these"*, and it is a genuinely good piece of
> objective design.
>
> Compare [[genetic-inverse-kinematics]]'s weighted sum, two pages later, where
> the trade-off *is* the intent. The lecture uses both forms correctly and
> discusses neither.

The `?` on `?V` is a shaping choice: it makes the penalty for small speed
differences **steeper** than linear, so near-straight motion is still punished and
the gradient towards perfectly straight is preserved. Unexplained in the source.

## What is striking about it

Nowhere does the fitness function mention **obstacle avoidance**, a goal, a path,
or a destination. It rewards *moving*, *straight*, *not near things* ? and
collision-free navigation is what **emerges** from optimising it.

> [!success] The clearest vindication in the module of L08's argument
> [[embodiment-and-situatedness]] and [[braitenberg-vehicle]] (L08) argued that
> complex behaviour can emerge from simple local rules coupled to a body and an
> environment, without a world model or a planner. Here that claim is put to work
> as an **engineering method**: specify three local quantities, evolve a
> controller, get navigation.
>
> It is also [[intelligent-behaviour|"intelligence is in the eye of the
> observer"]] made operational ? the observer calls it navigation; the system
> optimises wheel speeds.
>
> Three lectures apart, and neither references the other.

## The rules of thumb this example illustrates

> **Do not constrain the search space too much**, e.g. through very detailed
> fitness functions or very specialised and goal-oriented operators.
> **Let evolution do its job to find solutions.**

`?` is exactly that restraint in practice. A *detailed* fitness function would
have specified a route; this one specifies three properties of good motion and
leaves the behaviour to be discovered. See [[fitness-function]].

> **Pitfalls of implementation:** #individuals, mutation rate, etc.
> **Destructive mutation / recombination gets removed quickly. But: wrong choice
> usually leads to long evolution and needs a lot of time.**

## Unclear in the source

- What is being evolved is **not stated** ? weights of a neural controller, or
  parameters of something else? Given the lecture's theme it is presumably a
  network, but the genome is never described.
- `V` is glossed as *rotation speed* while `?V` is *difference of speeds of
  wheels*; whether `V` is the mean wheel speed or something else is unclear.
- No results, no environment description, no number of generations.

## See also

- [[fitness-function]] ? [[evolutionary-algorithm]] ? [[braitenberg-vehicle]] ?
  [[embodiment-and-situatedness]]
