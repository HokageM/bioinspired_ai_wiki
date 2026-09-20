---
title: Auditory attention model
type: system
sources: [L09]
tags: [attention, audition, architecture]
updated: 2026-09-21
---

# Auditory attention model

The pipeline given for [[auditory-scene-analysis|ASA]]:

```
Integrated sound ??units??? Grouping ??streams??? Segregation ??objects???
        Object competition ??? { Talker, Noise, Noise }
                    ?
             Top-Down-Attention
```

> **After segregation and competition, foreground sound stands out from the
> background noise.**
>
> **Top-down attention control can modulate processing on each stage.**

## Reading the stages

| Stage | In | Out | Job |
|---|---|---|---|
| **Grouping** | units | streams | bind energy that belongs together |
| **Segregation** | streams | objects | split what does not |
| **Object competition** | objects | one foreground | choose |

The last stage is [[winner-take-all]] again, over auditory objects rather than
image locations ? the module's fifth use of the same primitive.

## Pseudocode

```
def auditory_attention(mixture, goal):
    units   = decompose(mixture)                    # time-frequency units
    streams = group(units,       bias=topdown(goal))
    objects = segregate(streams, bias=topdown(goal))
    scores  = [salience(o) * topdown(goal, o) for o in objects]
    return objects[argmax(scores)]                  # foreground
```

The `bias=` argument on every line is the content of *"top-down attention
control can modulate processing on each stage"* ? and it is the strongest form
of top-down control in the module.

> [!note] Third and strongest instance of top-down modulation
> L07: cortex biases one stage ([[superior-colliculus|SC]] fusion). L08: a goal
> code enters one stage of [[object-picking-architecture|object picking]].
> **L09: the goal biases every stage of the pipeline.**
>
> That is a much stronger claim, because it means even *grouping* ? which sounds
> like a purely stimulus-driven operation ? is goal-dependent. What counts as
> one sound depends on what you are listening for. See [[top-down-modulation]].

> [!warning] The text contradicts the diagram
> Diagram: **Grouping ? Segregation**. Text on the same page: *"compound sound
> enters the bottom-up processing in the form of **segregated features** and then
> the features are **grouped into streams**"* ? the opposite order.
>
> Both orders are defensible as accounts (decompose-then-bind, or
> bind-then-split), but they are different architectures and the lecture asserts
> both within four lines. The wiki follows the diagram in the pipeline above
> because the diagram labels its arrows (*units*, *streams*, *objects*), which
> at least is internally consistent.

## What is missing

Everything operational: no grouping cue is named (not onset, not harmonicity,
not [[interaural-time-difference|ITD]] ? though L05 supplies ITD and it is the
obvious one), no competition dynamics, no evaluation, no relation to the
[[hybrid-acoustic-tracking]] robot of L05 that faced exactly this problem.

## See also

- [[auditory-scene-analysis]] ? [[winner-take-all]] ? [[top-down-modulation]]
- [[auditory-pathway]] ? [[cross-correlation-localisation]]
