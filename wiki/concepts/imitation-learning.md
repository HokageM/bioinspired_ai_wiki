---
title: Imitation learning
type: concept
sources: [L08]
tags: [robotics, learning, imitation-learning, agents]
updated: 2026-09-21
---

# Imitation learning

> - **Task demonstrations are shown to the robot and recorded**
> - **From the examples, objects and transitions are estimated**
> - **Robot infers inverse kinematics to perform tasks**

The pipeline:

```
Human Demo ??(DL planner?)??? Symbolic Reasoning ??? Action Plan
           ??(Inverse Kinematics)??? Robot Imitation
```

(The box before *Symbolic Reasoning* is annotated in the notes with something
close to *"DL Plann?"* and is not legible enough to transcribe.)

## Pseudocode

```
def imitate(video_of_human):
    objects, transitions = estimate(video_of_human)   # what changed, and how
    plan   = symbolic_reasoning(objects, transitions) # a symbolic action plan
    joints = inverse_kinematics(plan)
    execute(joints)
```

## The problem this raises for the lecture

> [!warning] This is [[functional-decomposition]] under a new name
> Perceive (estimate objects) ? model (symbolic representation) ? plan (action
> plan) ? execute (inverse kinematics). That is the four-stage classical
> pipeline that **page 1 of the same lecture rejects** as brittle, expensive,
> and subject to the granularity problem.
>
> And the granularity problem bites hardest exactly here: *at what level of
> detail should a demonstration be symbolised?* Too coarse and the plan is not
> executable; too fine and nothing transfers to a new object arrangement.
>
> The lecture presents both positions, nine pages apart, with no comment.

## The honest reading

The fork is not resolved because it **is not resolvable by slogan**. Reactive
architectures win where the task is *stay alive and keep moving*; symbolic
pipelines win where the task is *reproduce a structured sequence someone showed
you once*. A demonstration is a one-shot, temporally extended, compositional
thing; nothing in [[behaviour-coordination]] can represent it.

So [[L08-behaviour-based-robotics]] is really two lectures:
**(a)** you do not need a model, and **(b)** here is what we build when we do.
The wiki records both, and the absence of any bridge between them.

## Relation to L04

[[L04-embodied-language-processing]] used imitation for **grounding** ? the
[[imitation-network]] learned to produce what it perceived, tying a symbol to a
sensorimotor act. L08 uses it for **skill transfer** ? copying a task structure.

| | L04 | L08 |
|---|---|---|
| Copied | the signal (to ground it) | the task (to perform it) |
| Output | a representation | an action plan |
| Needs symbols | no | **yes** |

Same word, different operations. The module uses it for both without
distinguishing them.

## See also

- [[imitation-network]] ? [[task-inference-network]] ? [[nico]]
- [[functional-decomposition]] ? what this pipeline is
- [[learning-paradigms]]
