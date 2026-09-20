---
type: concept
tags: [multimodal, perception]
sources: [L07]
status: developing
---

# Modality appropriateness hypothesis

> **That modality is given full credence which is most appropriate for the
> stimulus.**
> ? [[L07-crossmodal-processing]], p2. Margin annotation: **Attention**.

The first of two explanations offered for the [[ventriloquism-effect]]. It says
perception picks a winner: for spatial judgements, trust vision; for temporal
judgements, trust audition.

## What it gets right

It accounts for the direction of every effect in the lecture:

| Judgement | Most appropriate modality | Observed |
|---|---|---|
| **where** | vision ? high spatial acuity | spatial ventriloquism: sound pulled to the light |
| **when** | audition ? high temporal acuity | temporal ventriloquism, double-flash illusion: vision pulled to the sound |

And the margin note **attention** points at a plausible mechanism: the
appropriate modality is the one attended, and attention gates what reaches the
percept.

## What it gets wrong

**"Full credence" is too strong.** Taken literally it predicts that the
non-dominant modality is ignored, so:

- capture should always be **complete**, never partial ? but it is graded;
- degrading the dominant modality should change nothing ? but blurring a visual
  target shifts the percept back toward the sound;
- there is no account of **why** one modality is appropriate, beyond the
  observation that it usually wins.

[[optimal-cue-integration]] fixes all three at once by replacing "most
appropriate" with **inverse variance**. The modality with the smaller error gets
the larger weight; when its error is very small the weight approaches 1 and the
behaviour looks like full credence. So modality appropriateness is not a rival
hypothesis ? it is **what optimal integration looks like from the outside**, in
the regime where one cue is much better than the other.

The lecture presents the two as alternatives and does not make this point.

## And the module's own thesis contradicts it

[[multisensory-integration]] is defined on the previous page as being

> **more powerful than just using the most appropriate modality.**

which is a direct rejection of this hypothesis. The lecture states the thesis on
p1, states the hypothesis on p2, and never notes the conflict.

## Pseudocode

```
function modality_appropriateness(cues, judgement_type):
    best = argmax over cue of appropriateness(cue, judgement_type)
    return cues[best]              # winner takes all; the rest is discarded

# contrast: optimal integration never discards anything
```

## Unclear in the source

- **"Appropriate" is never defined** or operationalised. Appropriateness is
  inferred from which modality wins, which makes the hypothesis close to
  circular as stated.
- **Attention** is written in the margin with an underline and no explanation.
  Whether attention is proposed as the mechanism, a synonym, or a separate
  factor is unclear.
- No account of what happens with **three or more** modalities.

## Related

[[ventriloquism-effect]] ? [[optimal-cue-integration]] ?
[[multisensory-integration]] ? [[unity-assumption]] ?
[[L07-crossmodal-processing]]
