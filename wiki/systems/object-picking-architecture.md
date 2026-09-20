---
title: Bio-inspired object-picking architecture
type: system
sources: [L08]
tags: [robotics, vision, architecture, hierarchy]
updated: 2026-09-21
---

# Bio-inspired object-picking architecture

## The spectrum it sits on

> **Object picking: spectrum of approaches ? modular vs end-to-end**

| Modular ???????????????????????????? End-to-end |
|---|

| Modular | Two-stage | One-stage |
|---|---|---|
| Object detection | Neural object detection | **Neural object picking** |
| Pose estimation | Non-neural attention focus | ? unified architecture |
| Inverse kinematics | Neural end-to-end grasping | ? end-to-end learning of object location, distribution and required kinematics |
| *(Neural end-to-end grasping: single object grasping)* | | |

This is the **same axis** as [[functional-decomposition]] versus
[[subsumption-architecture]] from the first half of the lecture ? hand-specified
stages against one coupled system ? now asked of a *learned* pipeline rather
than a hand-written one. The lecture presents it as a new topic.

## The architecture

**Components:**

> - **Visual processing** ? low-level visual processing ? pattern & shape processing
> - **Goal encoding**
> - **Visuomotor processing** ? task-relevant spatial information ? motor control

**Flow:**

```
Low-level hierarchical visual processing
            ?
   Pattern & shape processing   ???  Goal intention encoding
            ?                              ?
   Task-relevant spatial information
            ?
       Hand control
```

## Reading it

Stages 1?2 are the [[visual-pathway|visual hierarchy]] of L06 ? edges, then
shapes. Stage 3 is the **where**, stage 4 the **act**. The
[[two-visual-streams|two-streams]] structure is reproduced, with one addition
that neither L06 nor L07 has: **goal intention encoding injected partway up**,
so what the system extracts depends on what it is trying to do.

> [!note] That injection is [[top-down-modulation]] again
> L07 had cortex biasing [[superior-colliculus|collicular]] fusion. Here a goal
> representation biases visuomotor extraction. Both are *context modulating an
> earlier stage rather than driving it*, and this is the module's second
> instance ? the first outside a purely sensory setting.
>
> It is also the thing [[reactive-agent|purely reactive]] architectures cannot
> do. Nine pages after arguing that goals need not be represented, the lecture
> draws a box labelled *goal intention encoding*.

## Pseudocode

```
def pick(image, goal):
    low   = low_level_visual(image)              # edges, orientation  (V1-like)
    shape = pattern_shape(low)                   # object identity     (ventral)
    goal_code = encode_goal(goal)                # what we are trying to do
    spatial = task_relevant_spatial(shape, low, goal_code)   # dorsal, modulated
    return hand_control(spatial)
```

Compare the modular alternative, which is the same computation with the joints
specified rather than learned:

```
def pick_modular(image):
    box   = detect(image)
    pose  = estimate_pose(box)
    return inverse_kinematics(pose)      # analytic, brittle, no learning
```

## What is not said

- Where on the spectrum **this** architecture sits. It is drawn as four coupled
  neural stages with a goal input, which is neither fully modular nor
  end-to-end, and the lecture does not place it.
- How goal intention is represented, or where it comes from.
- Whether the stages are trained jointly or separately ? the whole point of the
  spectrum.

## See also

- [[nico]] ? [[neural-grasp-learning]] ? [[levels-of-abstraction]]
- [[convolutional-network]] ? the obvious realisation of stages 1?2, not named here
