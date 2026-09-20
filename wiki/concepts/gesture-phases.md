---
title: Gesture phases
type: concept
sources: [L10]
tags: [gesture, dynamics, representation]
updated: 2026-09-21
---

# Gesture phases (Kendon)

> **Rest position ? Pre-stroke ? Stroke ? Post-stroke ? Rest position**

Annotated in the notes as *"snapshots, peaks"*.

A gesture is not a uniform movement. It has a **stereotyped temporal structure**,
and only one of its phases carries the meaning: the **stroke**. The pre- and
post-stroke phases are transport ? getting the hand there and bringing it back.

## Why this matters practically

It answers the challenge L10 raises for
[[static-and-dynamic-gestures|dynamic gestures]]: *"start and end of isolated or
continuous gestures"*. If every gesture begins and ends at rest, then **rest
positions are the segmentation boundaries**, and they are detectable without
knowing which gesture is being performed.

It also justifies **snapshot extraction**: if the meaning is concentrated at the
stroke, the informative frame is the one at the **peak** ? which is exactly what
[[snapshot-model]]'s static channel selects, via **peak detection** on the
[[motion-intensity-profile]].

```
profile = motion_intensity(frames)       # ISSIM against the first frame
peaks   = find_peaks(profile)            # candidate strokes
snaps   = [frames[p] for p in peaks]     # the informative postures
```

So the three ideas fit together, and the lecture presents them on the same page
without stating the chain:

**phases ? the stroke is the informative moment ? find it as a peak in the
motion profile ? classify that frame statically.**

> [!note] A behavioural prior doing an engineer's work
> This is one of the module's better bio-inspired arguments, and it is not
> flagged as one. The claim is not *this network resembles a brain* but *human
> movement has a known structure, so build a detector that assumes it*. Compare
> [[ann-brain-correspondence]]: this is a fourth kind of justification ?
> **structure of the signal**, rather than structure of the mechanism.

## Unclear

- **Kendon** is named with no first name, date or citation. See
  [[adam-kendon]].
- Whether *pre-stroke* and *post-stroke* here mean the preparation/retraction
  movements or the brief **holds** on either side of the stroke is not stated;
  the two are standard and distinct [external], and the notes' *"snapshots,
  peaks"* gloss does not settle it.

## See also

- [[motion-intensity-profile]] ? [[snapshot-model]] ? [[gesture-continuum]]
