---
title: Reactive agent
type: concept
sources: [L08]
tags: [robotics, agents, embodiment]
updated: 2026-09-21
---

# Reactive agent

An agent whose action is an **immediate function of current sensor values**,
with no world model, no plan and no internal state.

> **No cognitive processes. Agent is purely reactive. Simple architecture leads
> to robustness.**

```
action = f(sensors)        # that is the entire agent
```

The [[braitenberg-vehicle]] is the limiting case: `f` is a 2?2 matrix of wires.

## What being reactive buys

| Property | Because |
|---|---|
| **Fast reaction** | immediate mapping of sensory info onto motor actions ? no stage in between |
| **Robustness** | if one part fails, the robot may retain some behaviour (*competence*) |
| **Multiple goals** | behaviours run concurrently; several goals can be pursued at once |
| **Extensibility** | easy to add new parts on top |
| **Simplicity** | complexity derives from continuous interaction of simple modules with the environment and each other |
| **Computational tractability** | usually simple calculations, parallelisable |

## What it costs

| Problem | Statement |
|---|---|
| **Limited information** | agents without env models must have sufficient info from the local environment |
| **Non-local information** | how does the agent take into account non-local info? |
| **Learning globally** | difficult to make a reactive agent that learns globally |
| **Complex dynamics** | hard to engineer agents with large numbers of behaviours |

The two lists are one fact seen twice. **Everything gained is gained by not
having a model; everything lost is lost for the same reason.** A reactive agent
cannot act on what it cannot currently sense, and cannot be told about it either.

> [!note] "Complex behaviour may simply be the reflection of a complex
> environment"
> The lecture's strongest claim. If it is right, the apparent sophistication of
> an agent is not evidence about the agent ? it is evidence about the world it
> is in. This inverts the usual inference from behaviour to mechanism, and
> [[intelligent-behaviour]] (L01) is where the module first needed it.

## The unresolved one

**Learning globally** is not a minor entry on a disadvantages list. It is the
[[hebbian-learning|local-versus-global]] thread that has run since L02, now on
the motor side: a purely local sensor-to-motor rule has no access to the
long-horizon consequence of its action.

This is the precise problem reinforcement learning exists to solve, and
[[learning-paradigms]] has had RL defined and unused since L03. L08 is the
lecture where the gap is most conspicuous: an agent, in a world, with actions and
outcomes, and no mention of reward.

## See also

- [[braitenberg-vehicle]] ? [[subsumption-architecture]] ? [[motor-schema]]
- [[embodiment-and-situatedness]] ? why a reactive agent can get away with it
- [[functional-decomposition]] ? what it is a reaction against
