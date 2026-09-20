---
title: Braitenberg vehicle
type: system
sources: [L08, L11]
tags: [robotics, agents, embodiment, foundations]
updated: 2026-09-21
---

# Braitenberg vehicle

A **simple agent**: two sensors, two actuators, and wires between them.

> - **Sensors: measure stimulus at given point**
> - **Actuators: transform input values into motion**

Example wiring: *light sensor, more light = faster.*

That is the whole specification. Everything below follows from **which sensor is
wired to which motor**, and **with what sign**.

## The four vehicles

With **excitatory** connections (more stimulus ? more motor):

| Name | Wiring | Behaviour |
|---|---|---|
| **Fear** (a) | straight / ipsilateral | drives fast away from light, then **slows down** with distance |
| **Aggression** (b) | crossed / contralateral | drives fast **to** the light, hitting it at full speed |

> **Connection can also be inhibitory, i.e. stronger stimulus ? smaller input to
> actuator.**

With **inhibitory** connections ? and the notes' own comment, *behaviours both
"like" the source*:

| Name | Wiring | Behaviour |
|---|---|---|
| **Admires** (a) | straight, inhibitory | gets slower as it approaches the source until it stops |
| **Explores** (b) | crossed, inhibitory | slows down when near a source, but **turns away from it looking out for a stronger source** |

Four wires, four temperaments.

## Pseudocode

```
# A Braitenberg vehicle is a 2x2 matrix and a sign.
#   sensors: (sL, sR)     motors: (mL, mR)

def vehicle(sL, sR, crossed, inhibitory):
    gain = -1 if inhibitory else +1
    base = MAX_SPEED if inhibitory else 0        # inhibition subtracts from full speed
    if crossed:
        mL, mR = base + gain*sR, base + gain*sL  # aggression / explores
    else:
        mL, mR = base + gain*sL, base + gain*sR  # fear / admires
    return mL, mR

while True:
    sL, sR = read_light_sensors()
    drive(*vehicle(sL, sR, crossed, inhibitory))
```

Why straight wiring turns *away*: the sensor nearer the light reads higher, so
the motor on **that same side** spins faster, and a differential-drive robot
turns towards its slower wheel ? i.e. away from the light. Crossing the wires
inverts this. ? (checked against the *slow / fast* labels in the notes' diagrams)

## Why this matters

> **Different stimuli (e.g. temperature, oxygen) ? vehicles will exhibit complex
> and dynamic behaviour.**
>
> **Behaviour is goal-directed, fast, flexible and adaptive.**
> **Might even appear intelligent.**
> **No cognitive processes. Agent is purely reactive.**
> **Simple architecture leads to robustness.**

Every word in the first line is a word one would normally take as evidence of an
internal model ? *goal-directed*, *adaptive*, *flexible*. None of them is earned
by anything inside the vehicle. They are earned by the **interaction** between
four wires and a structured environment.

> [!note] This is the hardest case for L01's definition of intelligence
> [[intelligent-behaviour]] took the position that behaviour is what we can
> observe and mechanism is what we infer. The Braitenberg vehicle is the
> counter-example that makes the inference unsafe: the behaviour supports rich
> mentalistic description and the mechanism supports none of it.
> [[reactive-agent]] carries the general form of the argument.

## Limits

Nothing here is **learned**. The wiring is chosen by the designer; the vehicle
has no mechanism to change it. The module has had local learning rules since L02
([[hebbian-learning]], [[stdp]]) that could in principle adapt these weights, and
L08 does not connect them.

Nor is there any memory: *admires* stops at the source and *explores* leaves it,
but neither can represent *a source I already visited*. The
[[local-minima-problem|avoid-past schema]] introduced later is the first
admission that some state is needed.

## See also

- [[reactive-agent]] ? [[embodiment-and-situatedness]]
- [[subsumption-architecture]] ? what you build when one vehicle is not enough
- [[excitatory-and-inhibitory-neurons]] ? the same two signs, in the brain
- [[valentino-braitenberg]]


## L11 ? the same behaviour, evolved rather than wired

[[collision-free-navigation]] (L11) produces obstacle-avoiding navigation from a
fitness function with three terms and no mention of obstacles as a goal:

`? = V (1 ? ??V)(1 ? i)` ? *go fast, go straight, keep sensors quiet.*

Braitenberg wired sensor to motor directly and showed that *fear*, *aggression*
and *love* are in the observer's description, not the vehicle. L11 declines to
wire anything and **searches** for the connection weights, using a measure of
good motion rather than a specification of the behaviour.

| | Braitenberg (L08) | Evolved navigation (L11) |
|---|---|---|
| Sensor?motor mapping | **specified by hand** | **found by search** |
| Designer supplies | the wiring | a quality measure |
| Behaviour is | a consequence of wiring | a consequence of optimisation |
| Observer says | *"it fears the light"* | *"it navigates"* |

> [!note] Same claim, upgraded from demonstration to method
> Braitenberg's point is that complex-looking behaviour needs no complex
> internals. L11 turns it into a procedure: if the behaviour is not in the
> internals, you do not have to design the internals ? you can search for them,
> given only a way to score the motion.
>
> This is the strongest support in the module for
> [[intelligent-behaviour|"intelligence is in the eye of the observer"]], and it
> arrives three lectures later with no reference back.
