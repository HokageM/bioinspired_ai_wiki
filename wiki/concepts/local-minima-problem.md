---
title: Local minima problem (in behaviour-based navigation)
type: concept
sources: [L08, L11]
tags: [robotics, navigation, contradiction]
updated: 2026-09-21
---

# Local minima problem

The two documented failure modes of [[motor-schema]] navigation.

| Problem | Symptom | **Possible solution** (the notes' hedge) |
|---|---|---|
| **Local minima** | robot stalls short of the goal, where attraction and repulsion cancel | **noise schema** |
| **Cyclic behaviour** | robot retraces the same loop forever | **avoid-past schema** |

The notes' first sketch shows a robot arriving at an obstacle and stopping dead
between it and the goal; the second shows the same robot wandering *around* the
obstacle once noise is added. The third shows a robot circling a step-shaped
obstacle; the fourth shows it escaping once past positions repel.

## Why it happens

It is not a bug in any schema. It is a property of **summing vectors**:

```
towards_goal  = ( +1,  0 ) * G_goal
avoid_obstacle= ( -1,  0 ) * G_obs      # obstacle sits between robot and goal
                ----------
total         = (  0,  0 )              # stall
```

Any scheme whose `C` is a **linear combination** can produce the zero vector
from non-zero parts. [[subsumption-architecture]] cannot stall this way ? the
winning layer's output is passed through unmodified ? which is the concrete
advantage of competitive [[behaviour-coordination]] and the reason the choice
of `C` is a real design decision rather than a taste.

## Why the fixes are not fixes

**Noise schema.** Adds a random vector each cycle, so the stall point is not
stable. But noise has no notion of *which way out*, the escape time is
unbounded, and enough noise to escape a deep minimum is enough to degrade normal
navigation. It converts a deterministic failure into a probabilistic one.

**Avoid-past schema.** Makes recently visited positions repulsive. This works ?
and it **breaks the architecture's premise**. A schema that avoids the past must
*remember* the past, so the agent is no longer purely reactive. The notes list
*"can have internal state"* for motor schemas without connecting it to this.

> [!note] The admission is structural
> [[reactive-agent]]'s disadvantage list already says **non-local information**
> is the open problem: *how does the agent take into account non-local info?*
> A local minimum is precisely a situation where the locally available
> information is insufficient, and the avoid-past schema is the module quietly
> re-introducing a world model ? the smallest possible one, a memory of where
> the robot has been.

```
# Noise schema
def noise():        return Vector(random_angle(), NOISE_GAIN)   # no state

# Avoid-past schema
visited = deque(maxlen=K)                                       # state!
def avoid_past(p):
    return sum(repulse(p, q) for q in visited)
```

## The same shape elsewhere

[[backpropagation|gradient descent]] (L03) has identical pathology on the **weight** surface,
and identical remedies ? noise, restarts, momentum. See
[[potential-field-navigation]] for the correspondence laid out. The module
presents them as unrelated practical annoyances in two different lectures.

## See also

- [[motor-schema]] ? [[potential-field-navigation]] ? [[behaviour-coordination]]


## L11 ? the same problem, in high dimensions, with a population

[[fitness-landscape]] restates this problem for search rather than navigation:

> **Local optimum:** solution better than neighbouring solutions.
> **Global optimum:** best solution in landscape.
> **Uni-modal:** only one local optimum. **Multi-modal:** several local optima.
> Usually of **very high dimension**.

| | L08 ? potential fields | L11 ? fitness landscape |
|---|---|---|
| Space | 2-D, drawable | *"very high dimension"* |
| Function | hand-designed by an engineer | the task's own quality measure |
| Agent | **one** robot | a **population** |
| Escape | noise, random walk, extra behaviour | weak parents, mutation, **recombination** |

> [!note] The population is the new idea
> L08's remedies perturb a single point and hope. An EA occupies several basins
> at once and can **combine** points from different basins, landing somewhere
> neither occupied ? which is why [[recombination]] is described as *"often a
> destructive jump in fitness landscape"*. Non-local moves and destructiveness
> are the same property.

> [!note] And the cure has its own disease
> Too much [[selection-pressure]] and the population collapses into one basin
> before it has explored ? [[premature-convergence]], which is the local-minimum
> problem returning at the level of the population rather than the individual.
> Three lectures apart, neither referencing the other.
