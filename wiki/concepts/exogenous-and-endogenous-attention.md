---
title: Exogenous and endogenous attention
type: concept
sources: [L09]
tags: [attention, neuroscience]
updated: 2026-09-21
---

# Exogenous and endogenous attention

> **Dichotomy between bottom-up (exo) and top-down (endo) control.**

Both are routed through the **orienting network**:

```
                Orienting network
                 ?            ?
      Exogenous attention   Endogenous attention
       (bottom-up)            (top-down)
       stimulus-driven        goal-driven
       external stimulus      internal / voluntary goal
       [verbal network]       [dorsal network]
```

(The two bracketed labels are marginal annotations in the notes and are not
explained.)

| | Exogenous | Endogenous |
|---|---|---|
| Trigger | the stimulus | the goal |
| Character | **instinctive and spontaneous** | **spotlight**, allocates limited resources |
| Control | involuntary | voluntary |
| Speed | fast | slower |

## The cocktail party shows both at once

> **At a noisy party, a person can concentrate on the target conversation (a
> top-down process) and easily respond to someone calling his/her name (a
> bottom-up process).**

This is the single best example in the module, because it shows the two are not
alternatives: the endogenous spotlight is held on one conversation *while* the
exogenous channel stays open enough to be interrupted by your own name. Selection
is therefore not a hard filter ? something is still being processed in the
"ignored" streams, or the interrupt could never fire.

The lecture does not draw that conclusion, and it bears directly on the
definition of [[attention|selective attention]] as *a filter*.

## The same fork, elsewhere in the module

| Lecture | Bottom-up pole | Top-down pole |
|---|---|---|
| L07 | [[superior-colliculus]] fusion | cortical [[top-down-modulation|modulation]] |
| L08 | [[reactive-agent]] | [[functional-decomposition]], goal encoding |
| **L09** | **exogenous attention** | **endogenous attention** |

Three lectures, three vocabularies, one distinction: *driven by what is there*
versus *driven by what you want*. L08's version is the strongest claim ? that
you can build a useful agent with **only** the bottom-up half ? and L09's
cocktail party is the clearest demonstration that a **biological** system runs
both simultaneously.

```
# The two, as control flow
def exogenous(scene):        return argmax(saliency(scene))      # the world picks
def endogenous(scene, goal): return argmax(saliency(scene) * bias(goal))  # you pick
```

Note that endogenous attention is implementable as a **reweighting of the same
saliency map** ? which is exactly how [[saliency-model|saliency models]] and
[[auditory-attention-model|ASA]] both do it. One mechanism, two sources of
weight.

## See also

- [[attention]] ? [[attention-networks]] ? [[auditory-scene-analysis]]
- [[top-down-modulation]]
