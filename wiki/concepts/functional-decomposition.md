---
title: Functional decomposition
type: concept
sources: [L08]
tags: [robotics, architecture, agents]
updated: 2026-09-21
---

# Functional decomposition

The classical approach to robot control: **top-down decomposition into logical
units**, each a stage in a pipeline.

| # | Unit | Stage |
|---|---|---|
| 1 | Processing sensory data | **Perception** |
| 2 | Creation of world model | **Modelling** |
| 3 | Planning | **Planning** |
| 4 | Plan execution | **Execution** |

Architecturally: **Sensors ? Robot Controller ? Actuators**, with all four
stages inside the controller, running in series, once per cycle ? the
*Move ? Think ? Next Move* loop.

## Why it is appealing

**(+) precise ? (+) controllable ? (+) predictable.**

These are not small virtues. Every stage has a specification, can be tested in
isolation, and the system's behaviour can be reasoned about. It is how one would
build any other piece of engineering.

## Why it fails on robots

**(?) difficult to handle noise and uncertainty** ? the world model is built
once per cycle from noisy sensors and then *trusted* by everything downstream.

**(?) unit failure ? system failure** ? a serial chain has no redundancy. Lose
perception and the robot is blind, deaf and still.

**(?) computationally expensive** ? modelling and planning are the two most
expensive things one can do, and they sit between the sensor and the motor.

**(?) granularity problem (model)** ? how detailed should the world model be?
Too coarse and planning fails; too fine and it cannot be built in time. There is
no principled answer, and the question is unavoidable the moment you decide to
have a model at all.

## The diagnosis

All four faults have the same cause: **the pipeline puts a model between sensing
and acting.** [[behaviour-coordination|Behaviour-based]] architectures remove
it. Brooks's slogan version is that *the world is its own best model* ? an idea
the notes render as **"absolute coordinate system leads to cumulative errors"**.

```
# Functional decomposition
while True:
    data  = perceive(sensors)
    model = update_world_model(model, data)    # expensive, and trusted
    plan  = plan(model, goal)                  # expensive, and stale
    execute(plan)                              # by now the world has moved
```

```
# Behaviour-based
while True:
    S = read_sensors()
    actuate(C(G, [b(S) for b in behaviours]))  # no model, no plan
```

## The lecture argues against it and then uses it

[[L08-behaviour-based-robotics]] opens by rejecting functional decomposition and
closes with an [[imitation-learning]] pipeline ?
*Human Demo ? Symbolic Reasoning ? Action Plan ? Inverse Kinematics ? Robot
Imitation* ? which is perception ? modelling ? planning ? execution under
different labels. The same axis reappears as
[[object-picking-architecture|modular vs end-to-end]].

The honest reading is that the fork is **not settled**, and the lecture's own
later material concedes it: symbolic reasoning over a demonstration is exactly
the thing reactive architectures cannot do.

## See also

- [[subsumption-architecture]] ? decomposition by **task**, not by function
- [[reactive-agent]] ? what is left when the model is removed
- [[levels-of-abstraction]] ? the module's other decomposition question
