---
title: Subsumption architecture
type: system
sources: [L08]
tags: [robotics, agents, architecture]
updated: 2026-09-21
---

# Subsumption architecture

Brooks's layered, competitive design.

> **Decompose robot control in terms of task-achieving behaviours rather than
> functional modules ([[functional-decomposition|functional decomposition]]).**
>
> **Complex behaviour may simply be the reflection of a complex environment.**

## Properties

- **Layered.**
- **Higher level implies more specific behaviour.**
- **All layers have access to sensors and outputs of lower-level modules.**
- **Higher level can inhibit or subsume output of lower levels, or reset state of
  module.**
- **Bottom-up development** ? *next level only when last level works*.
- **Widely used in real-time AI.**

The example stack, low ? high:

```
Sensor
  ??? Identify
  ??? Happy(?)          # the notes' handwriting is ambiguous here
  ??? Explore
  ??? Wander
  ??? Avoidance
Actuators
```

with the notes' marginal annotation showing the higher layers reaching down into
the lower ones.

## The decomposition, contrasted

| | Functional | Subsumption |
|---|---|---|
| Slices by | **stage of processing** | **task achieved** |
| A slice is | perception, modelling, planning? | *avoid obstacles*, *wander*, *explore* |
| One slice alone | does nothing useful | **is a working robot** |
| A slice fails | system stops | robot degrades to the layers below |

The last row is the entire argument. Each layer is a complete
sensor-to-actuator path, so a stack of `k` layers is `k` nested robots, each of
which works. Losing the top gives you the robot you had before you added it.

That is also why **bottom-up development** is not merely a methodology
preference: you can only build layer `k+1` once layer `k` runs, because layer
`k+1` is defined by what it suppresses.

## Pseudocode

```
# Each layer is a complete robot.
layers = [avoidance, wander, explore, identify]     # low -> high

while True:
    S = read_sensors()
    out = REST
    for layer in layers:                 # low to high; later layers win
        proposal = layer(S, lower_outputs=out)
        if layer.active(S):
            out = suppress(out, proposal)   # higher subsumes lower
    actuate(out)
```

```
# Suppression / inhibition, the two operators in the diagram
def suppress(low, high):  return high          # replace the signal
def inhibit(low):         return REST          # block it, supply nothing
```

> [!warning] Suppression timing is the hard part and is not mentioned
> A real suppression node holds a lower layer down for a **fixed interval** after
> the last high-level message; get that interval wrong and the robot either
> oscillates between layers or goes deaf to obstacles. The notes show the `S`
> nodes and say nothing about duration. This is the single most implementation-
> critical detail of the architecture.

## Relation to the rest of the module

- Coordination scheme: **competitive**. See [[behaviour-coordination]].
- The opposite choice ? blend everything ? is [[motor-schema]].
- **Inhibition as an architectural primitive** appears here, in
  [[braitenberg-vehicle]]s, in [[excitatory-and-inhibitory-neurons]] (L02) and in
  [[top-down-modulation]] (L07). L07's is the odd one out: cortex *modulates*
  the colliculus rather than suppressing it, which is a strictly weaker and more
  flexible coupling than subsumption's replace-the-signal.

> [!note] Higher levels here are *more specific*, not *more abstract*
> In the visual hierarchy of [[levels-of-abstraction|L06]], going up means more
> abstract and more invariant. In subsumption, going up means **more specific
> behaviour**, and the lowest layer is the most general (avoid everything,
> always). The two hierarchies run in opposite directions, and the module uses
> the same word for both.

## See also

- [[rodney-brooks]] ? [[reactive-agent]] ? [[embodiment-and-situatedness]]
