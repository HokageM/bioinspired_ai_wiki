---
type: concept
tags: [multimodal, neuroscience, foundations]
sources: [L07, L13]
status: solid
---

# Multisensory integration (MSI)

> **The process by which information from different sensory modalities is
> combined to yield a rich, coherent representation of an object or event in the
> environment.**
> ? [[L07-crossmodal-processing]], p1

> **More powerful than just using the most appropriate modality.**

That second line is the lecture's thesis and the reason
[[modality-appropriateness-hypothesis]] is introduced only to be superseded.
Picking the best sense discards information; combining them does not.

## What it buys

- **Disambiguation of object properties** ? one modality resolves what another
  leaves ambiguous.
- **Increased accuracy**, and a way to **deal with redundancy** in understanding
  events.
- **A coherent, robust and efficient interaction with the environment.**

The engineering goal stated in the notes: **embedding MSI in artificial agents
and robots acting in the world.**

## The terminology problem

The same phenomenon has four names depending on who is studying it. The lecture
opens by laying them out, which is unusually careful:

| Term | Field | Definition given |
|---|---|---|
| **Crossmodal integration** | interdisciplinary | behavioural tasks involving one or several senses |
| **Multimodal integration** | AI, intelligent systems | integration of multiple knowledge-based modalities |
| **Multisensory integration** | cognitive neuroscience | activity of neurons responding to one sense or more; neural processes synthesising information from different stimuli |
| **Multisensory fusion** | engineering, robotics | mapping several objects to a single object; combining information into a larger system |

Read down the definitions and the discipline shows through. The neuroscience
definition is about *neurons*, the AI one about *knowledge*, the robotics one
about *objects*, the interdisciplinary one about *tasks*. Each field defines the
process at the level it can measure.

## The three questions MSI has to answer

Implicit in the rest of the lecture, never stated as a list:

1. **Should these signals be combined at all?** ? [[unity-assumption]]
2. **If so, in what proportion?** ? [[optimal-cue-integration]],
   [[inverse-effectiveness]]
3. **Where in the system does it happen?** ? [[superior-colliculus]],
   [[fusion-strategies]]

## Pseudocode

```
function msi(x_visual, x_auditory):
    # 1. should they be bound? (unity assumption)
    if not consistent(x_visual, x_auditory):
        return segregate(x_visual, x_auditory)   # two separate percepts

    # 2. combine, weighted by reliability
    w_v = reliability(x_visual)
    w_a = reliability(x_auditory)
    return (w_v * x_visual + w_a * x_auditory) / (w_v + w_a)
```

The whole lecture is an elaboration of these two steps: p4 is step 1, p2 and p5
are step 2, and p3/p6 are about where in an architecture to put them.

## Related

[[ventriloquism-effect]] ? [[modality-appropriateness-hypothesis]] ?
[[optimal-cue-integration]] ? [[unity-assumption]] ? [[spatial-principle]] ?
[[inverse-effectiveness]] ? [[superior-colliculus]] ? [[fusion-strategies]] ?
[[cross-modal-stimuli-prediction]] ? [[symbol-grounding]] ?
[[L07-crossmodal-processing]]

## L13 — crossmodal learning as a continual-learning paradigm

Listed as the fourth paradigm related to [[continual-learning]]:

> - **Multisensory integration and crossmodal enhancement**
> - **Dynamic process across a lifespan**
> - **Address sensory uncertainty and conflict resolution**

Diagram: two modalities with **integration** upward and **enhancement** sideways
between them.

The new word is **lifespan**. L07 treated integration as a fixed computation over
two streams; L13 makes the integration itself something that **develops and keeps
changing** — the weighting between modalities is learned from experience and
re-learned when bodies, sensors or environments change.

That converts [[L07-crossmodal-processing]]'s reliability weighting from a
parameter into a **continually estimated** quantity, which is the only version
that could work in a system whose sensors drift.
