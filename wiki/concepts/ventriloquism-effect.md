---
type: concept
tags: [multimodal, perception, neuroscience]
sources: [L07, L09]
status: solid
---

# Ventriloquism effect

> **Visual influence on auditory perception.**
> **Visual capture of a talking puppet causes perception of sound direction to
> change.**
> ? [[L07-crossmodal-processing]], p2

The classic demonstration that the senses are not independent channels. You know
the sound comes from the performer; you hear it from the puppet anyway. The
effect is not a belief, it is a percept, and it is not optional.

## Three variants

| Variant | Effect | Which sense wins |
|---|---|---|
| **Spatial ventriloquism** | localisation at the position of the **visual** event | vision |
| **Temporal ventriloquism** | **flash perceived closer in time** than in reality | audition |
| **Double-flash illusion** | **2 flashes and 2 noise bursts perceived although there was only one flash** | audition |

The table is the lecture's most important observation, though it does not say so:
**vision does not simply dominate.** In two of the three cases *sound alters
vision*. What determines the winner is the **dimension in question** ? space or
time ? not the modality.

This is exactly what makes the naive reading of the
[[modality-appropriateness-hypothesis]] work, and exactly what
[[optimal-cue-integration]] explains better: vision has low spatial variance and
poor temporal resolution; audition is the reverse.

## Two explanations, offered as rivals

**1. [[modality-appropriateness-hypothesis]]** ? *that modality is given full
credence which is most appropriate for the stimulus.* Margin note: **attention**.

**2. Integration** ? *small errors in visual localisation and large errors in
auditory localisation lead to optimal localisation being very close to the visual
estimate, hence the ventriloquism effect.*

The second is strictly stronger. It derives the effect rather than stipulating
it, and it predicts the *magnitude* of capture ? including the cases where
capture is partial, which "full credence" cannot accommodate at all. See
[[optimal-cue-integration]].

## Pseudocode

```
# What "full credence" predicts (modality appropriateness):
percept_location = visual_location            # audition ignored entirely

# What integration predicts:
w_v = 1 / var_visual
w_a = 1 / var_auditory
percept_location = (w_v * visual_location + w_a * auditory_location) / (w_v + w_a)

# With var_visual << var_auditory these agree, which is why the effect
# looks like capture. They diverge when the two cues are far apart, or
# when visual reliability is degraded (e.g. a blurred light source):
# integration predicts the percept shifts back toward the sound.
# Full credence predicts no change at all.
```

## Unclear in the source

- The two hypotheses are never adjudicated, and no experiment distinguishing
  them is described.
- **No citations** for any of the three effects, despite each being a specific,
  famous experimental result. `[external]` The double-flash illusion is due to
  Shams, Kamitani and Shimojo (2000); the notes name no one.
- The **unity assumption** appears two pages later and is not connected back to
  ventriloquism, though it is what licenses the binding in the first place ? the
  puppet and the voice are captured *because* they are judged to share an origin.

## Related

[[modality-appropriateness-hypothesis]] ? [[optimal-cue-integration]] ?
[[multisensory-integration]] ? [[unity-assumption]] ? [[spatial-principle]] ?
[[azimuth-and-elevation]] ? [[geometric-sound-localisation]] ?
[[L07-crossmodal-processing]]


## A third explanation (L09)

[[L09-bio-inspired-attention]] restates the effect and files it under
**audiovisual crossmodal selective *attention***:

> **Auditory stimulus is perceptually shifted towards the position of the
> synchronous visual stimulus.**
> **Distance between audiovisual stimuli is crucial for sound localisation.**

The second line is the [[spatial-principle]] under another name — and is the
one piece of L07 machinery L09 reproduces.

> [!warning] The module now has three accounts and has adjudicated none
> | Account | Lecture | Mechanism |
> |---|---|---|
> | [[modality-appropriateness-hypothesis|Modality appropriateness]] | L07 | vision is the spatial modality; it wins |
> | [[optimal-cue-integration]] | L07 | vision is more reliable in space; it gets the weight |
> | **Attentional capture** | **L09** | the visual event **draws attention**, and the percept follows |
>
> These are not notational variants. The first two are **perceptual**: fusion
> happens whether or not you are attending, and the shift is a property of the
> estimator. The third is **attentional**: the shift is downstream of where you
> looked, and should therefore be manipulable by instructing someone to attend
> elsewhere — which the first two predict would change little.
>
> That is a testable difference, and it is the obvious experiment neither
> lecture proposes.
