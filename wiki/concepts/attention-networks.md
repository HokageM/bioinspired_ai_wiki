---
title: The three attention networks
type: concept
sources: [L09]
tags: [attention, neuroscience, anatomy]
updated: 2026-09-21
---

# The three attention networks

The **functional neural network model** of attention.

| Network | Function (source) | Areas (source) |
|---|---|---|
| **Alerting** | **ability to be alert for upcoming stimuli** | frontal lobe, thalamus, parietal lobe |
| **Orienting** | **focus on specific information among multiple sensory input** | TPJ, SPL, FEF |
| **Executive control** | **monitoring and resolving conflicts between different inputs** | ACC, dlPFC, AI |

These are **functional** divisions with anatomical correlates, not anatomical
divisions ? each spans several areas, and the areas are not contiguous.

## What each one is for

**Alerting** is not about *what* to attend to but about *being ready at all*. It
is the only one of the three that is not selective: raising alertness raises
readiness for everything.

**Orienting** is the selector, and is where the
[[exogenous-and-endogenous-attention|exogenous/endogenous dichotomy]] lives ?
both modes are drawn as branches of the orienting network.

**Executive control** is the only one that presupposes a **conflict**. It is
needed exactly when two inputs demand incompatible responses, which is why it is
measured by congruent-versus-incongruent trials in the
[[attention-network-test]].

> [!note] These are the three functions L08's robot has none of
> A [[reactive-agent]] has no alertness state (it always reacts identically), no
> endogenous orienting (no goal), and no conflict monitoring ? its
> [[behaviour-coordination]] resolves conflict by a fixed rule rather than by
> monitoring. The three attention networks are, almost exactly, the list of what
> behaviour-based robotics leaves out.

## Abbreviations

> [!warning] Expanded nowhere in the module
> TPJ, SPL, FEF, ACC, dlPFC, AI. The wiki does not gloss them because the source
> does not, and guessing would violate the anti-invention rule (`CLAUDE.md` ?6).
> [external] They are standard cognitive-neuroscience abbreviations; a reader
> will need a textbook, not this wiki.

## Measurement

Each network gets one difference score in the
[[attention-network-test|Attentional Network Test]]:

```
alerting  = RT(no cue)      - RT(double cue)
orienting = RT(centre cue)  - RT(spatial cue)
executive = RT(incongruent) - RT(congruent)
```

The design intent is that one task yields three independent measures ? which is
only valid if the three networks really are separable stages, which is what the
[[additive-factors-method]] is supposed to establish.

## See also

- [[attention]] ? [[attention-network-test]] ? [[reaction-time]]
- [[visual-pathway]] ? FEF and SPL sit in the L06 dorsal stream
