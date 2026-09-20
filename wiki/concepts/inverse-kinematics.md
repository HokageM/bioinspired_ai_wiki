---
title: Forward and inverse kinematics
type: concept
sources: [L11]
tags: [robotics, optimisation]
updated: 2026-09-21
---

# Forward and inverse kinematics

> **Robotic manipulator:**
> - joints `?j? ? j??`
> - pose of end effector ? **position in 3D space** `(x, y, z)` **+ rotation in
>   3D space** `(?, ?, ?)`
>
> **Joint configuration ? pose of end effector: forward kinematics**
> **Pose ? joint configuration: inverse kinematics** *(marginal: "hard!")*

## Why one direction is easy and the other is not

**Forward** is a composition of rigid transforms: given the angles, chain them
and read off the result. One answer, always, computed directly.

**Inverse** is the inverse of that map, and it misbehaves in three ways:

| Problem | What it means |
|---|---|
| **Many solutions** | a 6-joint arm can usually reach the same pose several ways ? elbow up or elbow down |
| **No solution** | poses outside the workspace have no configuration at all |
| **No closed form** | for general geometries the equations are not analytically solvable |

So it is a **search** problem, not a calculation ? which is why
[[genetic-inverse-kinematics|a genetic algorithm]] is a reasonable thing to point
at it. The search space is the 6-dimensional joint space, the objective is
distance from the goal pose, and no gradient is required.

> [!note] The module's cleanest example of a well-posed optimisation problem
> Unlike every neural task in the previous ten lectures, this one has an
> objective that is **known exactly and computable in closed form**: run forward
> kinematics on a candidate and measure the error. No training data, no labels,
> no generalisation question. Evaluation is exact and cheap.
>
> That makes it an unusually honest setting for an EA ? and it also means the
> comparison against dedicated numerical solvers is a fair fight, which is
> presumably why the lecture's cons are as blunt as they are.

## Connection to L08 and L10

[[object-picking-architecture]] (L08) had a robot reach for objects and
[[neural-grasp-learning]] learned the mapping from a neural network instead.
[[human-pose-estimation]] (L10) recovers human joint configurations from images ?
which is inverse kinematics on a person, solved by a trained network.

Three approaches to the same class of problem across the module: **learn it**
(L08), **regress it from data** (L10), **search for it** (L11). No lecture sets
them side by side.

## See also

- [[genetic-inverse-kinematics]] ? [[object-picking-architecture]] ?
  [[human-pose-estimation]]
