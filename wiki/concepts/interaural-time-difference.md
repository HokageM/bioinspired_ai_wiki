---
type: concept
title: Interaural Time Difference (ITD)
sources: [L05]
tags: [audition, localisation, coding]
---

# Interaural Time Difference (ITD)

> **Time difference (ms) between arrival of the signal at the two ears.**

The primary cue for azimuth. **Difference in azimuth changes the time delay of
the signal arriving at the receivers** — a source off to the left reaches the
left ear first.

The p1 sketch shows two sources with `ITD(S2) > ITD(S1)`, and a plot of ITD
against angle running from `−90°` through `0°` to `+90°`, passing through zero
when the source is straight ahead.

## The physics

The extra distance the sound must travel to the far ear is

```
a = c · t_ITD
```

with `c` the speed of sound. Recovering the angle from `a` is
[[geometric-sound-localisation]].

ITD is **zero on the midline** and maximal at the sides — which means it is most
informative near the front and least informative at the extremes, and it cannot
by itself distinguish front from back. The notes do not raise the front/back
ambiguity.

## Scale

`[external]` For a human head (~15–18 cm between the ears) the maximum ITD is
about **0.6–0.7 ms**, and the smallest detectable difference is around **10 µs**.
That resolution is the reason this cue cannot be carried by a rate code: no
plausible firing rate resolves ten microseconds. It has to be spike timing.

> [!note] Units in the source
> The notes write "(ms)", which is the right order of magnitude for the full
> range but coarse — the literature normally quotes µs.

## Why it matters to the module

**This is the cue that justifies L02.** Two lectures of rate-coded networks
(L03, L04) work fine for images and words. They cannot work here.
[[temporal-coding]] was presented in L02 as one branch of a fork; L05 is the
first problem where the other branch is simply unavailable.

## How it is computed

| | Mechanism | Page |
|---|---|---|
| Algorithmically | sweep shifts, take the max dot product | [[cross-correlation-localisation]] |
| Classically, in neurons | delay lines + coincidence detectors | [[jeffress-model]] |
| Anatomically | **Medial Superior Olive (MSO)** | [[auditory-pathway]] |

All three are the same computation. See [[ann-brain-correspondence]].

## Duplex theory

ITD dominates judgement at **low frequencies**; [[interaural-level-difference]]
dominates at high frequencies. The reason is [[acoustic-shadow]] — and, for
ITD, the fact that at high frequencies the wavelength becomes shorter than the
head width, so a phase difference no longer identifies a unique delay.
`[external — the notes give the "what" but not this "why" for the ITD side.]`

## See also

[[interaural-level-difference]] · [[acoustic-shadow]] ·
[[geometric-sound-localisation]] · [[jeffress-model]] ·
[[cross-correlation-localisation]] · [[auditory-pathway]] ·
[[temporal-coding]] · [[azimuth-and-elevation]] ·
[[L05-robot-sound-localisation]]
