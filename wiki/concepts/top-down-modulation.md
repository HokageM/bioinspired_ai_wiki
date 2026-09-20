---
type: concept
tags: [multimodal, neuroscience, architecture]
sources: [L07, L08, L09, L12]
status: developing
---

# Top-down modulation

From [[L07-crossmodal-processing]] p6, the mechanism joining cortical and
subcortical [[multisensory-integration]].

> **Top-down: cortical. Bottom-up: collicular subcortical alignment.**

> **Multimodal units show the highest activation for congruent audiovisual
> patterns**, and that activity is used as a **top-down modulatory projection to
> bias the integration at the subcortical layer.**

## The loop

```
   cortex        multimodal units ? highest activation when congruent
      ?  ?
      ?  ? modulatory projection: "these signals agree, trust the fusion"
      ?  ?
   SC (subcortical)   integration biased accordingly
```

Bottom-up, the [[superior-colliculus]] fuses whatever arrives, fast and reflexive.
Top-down, cortex has a slower and better-informed opinion about whether the two
streams really belong to one event ? and rather than overriding the SC, it
**biases** it.

## Why this is the right shape

It solves a problem the [[unity-assumption]] leaves open. Deciding whether two
signals share a cause needs context ? what objects are present, what usually
makes that sound ? and that knowledge is cortical. But orienting has to be fast,
and that is subcortical. A modulatory projection lets the slow, knowledgeable
system tune the fast, reflexive one without sitting in its path.

Note what plays the role of the congruence signal: **activation level itself.**
Multimodal units fire hardest for congruent inputs, so their activity *is* a
congruence measure, available for free. The same trick appears in
[[optimal-cue-integration]], where the p6 summary proposes that *levels of neural
activity in the unimodal layer may provide the reliability of each modality.*

Twice on the same page, **activity magnitude is used as a confidence signal**
rather than as a content signal. The lecture never names this as a principle,
and it is one of the more interesting ideas in the module.

## Pseudocode

```
function cortico_collicular_step(v, a):
    # bottom-up: fast subcortical fusion
    sc = sc_integrate(v, a)

    # cortical multimodal units: highest activation when congruent
    cortical = multimodal_layer(net_v(v), net_a(a))
    congruence = activation_level(cortical)

    # top-down: bias, not override
    return sc_integrate(v, a, gain = f(congruence))
```

```
# gain > 1 when congruent  ? reinforce the fusion
# gain < 1 when incongruent ? let the modalities segregate
```

## Unclear in the source

- **"Modulatory" is not distinguished from "driving"** ? the notes do not say
  what makes a projection modulatory rather than an ordinary input, which is the
  crux of why it biases rather than dictates. `[external]`
- **No formula** for how congruence translates into bias.
- **The loop is never closed in a model.** The architecture is drawn; no training
  procedure or result is reported for it.
- Whether the top-down signal operates within a trial or across learning is not
  stated.

## Related

[[cortico-collicular-architecture]] ? [[superior-colliculus]] ?
[[multisensory-integration]] ? [[unity-assumption]] ?
[[optimal-cue-integration]] ? [[gated-multimodal-unit]] ?
[[hybrid-architecture]] ? [[L07-crossmodal-processing]]


## Second instance: goal intention encoding (L08)

[[object-picking-architecture]] injects a **goal intention encoding** into
visuomotor processing partway up the visual hierarchy, so that what counts as
*task-relevant spatial information* depends on what the robot is trying to do.

Structurally this is the same move as L07's cortico-collicular projection:
**context biases an earlier stage rather than driving it.**

| | L07 | L08 |
|---|---|---|
| Modulator | cortical congruence / [[unity-assumption]] | the current goal |
| Modulated | [[superior-colliculus|SC]] fusion | visuomotor spatial extraction |
| Coupling | modulatory, not driving | injected alongside, not replacing |

> [!note] And it is what a reactive agent cannot have
> A goal representation is exactly the internal state
> [[reactive-agent|purely reactive]] architectures forgo. The same lecture that
> argues goals need not be represented draws a box labelled *goal intention
> encoding*. See [[imitation-learning]] for the other half of this tension.


## Third and strongest instance (L09)

[[auditory-attention-model|Auditory scene analysis]] states it most broadly:

> **Top-down attention control can modulate processing on each stage.**

| Instance | Lecture | Scope of the bias |
|---|---|---|
| Cortico-collicular | L07 | one stage (fusion) |
| Goal intention encoding | L08 | one stage (visuomotor) |
| **ASA** | **L09** | **every stage — grouping, segregation, competition** |

The escalation matters. Biasing *competition* is unsurprising — that is just
choosing. Biasing **grouping** is a much stronger claim: what counts as **one
sound** would then depend on what you are listening for, so the goal reaches all
the way down into the construction of the objects themselves, before any
selection happens.

L09 also supplies the vocabulary the earlier instances lacked: top-down
modulation is **endogenous attention**. See
[[exogenous-and-endogenous-attention]].

## L12 ? a stack drawn without a downward arrow

L12's three-level architecture ? **(a) symbolic / (b) neural-statistical /
(c) sensory** ? is drawn with a single arrow pointing **upward**, from sensation
through sub-symbolic processing to symbols.

> [!warning] The module's own earlier lectures say this is wrong
> L07 and L09 spend most of their length establishing that higher levels
> **modulate** lower ones: task demands bias [[saliency-map|saliency]], context
> reshapes [[receptive-field|receptive fields]], goals set the gain on sensory
> channels. A purely feed-forward abstraction stack contradicts that directly.
>
> A symbolic level that cannot constrain the neural level below it also cannot do
> the thing hybrid systems are built for ? inject knowledge. And
> [[neural-symbolic-integration]]'s own taxonomy names **tight coupling
> (bidirectional)** two bullets later, so the diagram contradicts the text on the
> same page.
