---
title: Rodney Brooks
type: entity
status: stub-by-source
sources: [L08]
tags: [people, robotics, history]
updated: 2026-09-21
---

# Rodney Brooks

Named twice in [[L08-behaviour-based-robotics]] ? as **"Brooks assumptions"**
and as **"Subsumption Architecture (Brooks)"**. No first name, date, institution
or citation is given.

## What the module attributes to him

**The assumptions:**

> - Complex behaviour does **not** require a complex control system
> - Simple ? increase stability and robustness
> - Robots should be **autonomous** to survive long without human
> - Environment is 3D
> - **Absolute coordinate system leads to cumulative errors**

**The architecture:** [[subsumption-architecture]] ? decomposition by
task-achieving behaviour rather than by function, layered, bottom-up, with
higher levels inhibiting or subsuming lower ones.

**The position:** *complex behaviour may simply be the reflection of a complex
environment*, and hence no world model is needed.

## Why he matters to the module's argument

Brooks is the only figure in the module whose contribution is a **negative
claim** ? that a standard engineering method is wrong. Everyone else
([[donald-hebb]], [[lloyd-jeffress]], [[david-hubel]] and [[torsten-wiesel]],
[[kunihiko-fukushima]], [[yann-lecun]], [[teuvo-kohonen]]) contributed a
mechanism. Brooks contributed an argument against one.

That is also why he is the only one the lecture half-contradicts: see
[[imitation-learning]], where the rejected pipeline reappears.

> [!note] No dates anywhere
> [[L06-hierarchical-vision]] gave a datable lineage (Hubel & Wiesel ?
> Fukushima ? LeCun). L08 gives none, so the wiki cannot place subsumption
> relative to the neural-network chronology it clearly reacts against.

## See also

- [[subsumption-architecture]] ? [[reactive-agent]] ?
  [[embodiment-and-situatedness]] ? [[functional-decomposition]]
