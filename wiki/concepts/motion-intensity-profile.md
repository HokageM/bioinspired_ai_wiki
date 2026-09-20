---
title: Motion intensity profile
type: concept
sources: [L10]
tags: [gesture, vision, methods, dynamics]
updated: 2026-09-21
---

# Motion intensity profile

> **Challenge: how to analyse motion/pause carried in a movement sequence?**

**Solution:**

> **Structure Similarity (SSIM) index ? calculates the similarity between two
> frames using luminance, contrast and structure.**
>
> **SSIM can be inverted to find the change in motion across a timespan:**
> **ISSIM = inverted SSIM**, i.e. `ISSIM = 1 ? SSIM(I_i, I_0)`
>
> **The first frame of the sequence is used as a reference.**

## What it gives you

A **scalar curve over time** ? one number per frame ? summarising how far the
posture has departed from where it started.

The notes' two examples:

| Gesture | Curve |
|---|---|
| **"turn left"** | two peaks, with a dip between |
| **"OK"** | a single rise and fall |

And that difference is the point: two gestures that a per-frame classifier might
confuse have visibly different **temporal signatures**, and the number of peaks
alone separates them.

```
def motion_profile(frames):
    I0 = frames[0]
    return [1 - ssim(f, I0) for f in frames]     # ISSIM against the reference
```

> [!warning] The name does not match the formula
> Referencing **the first frame** makes this a *cumulative displacement from
> rest*, not an *intensity of motion*. A true intensity profile differences
> **consecutive** frames:
>
> ```
> intensity[i] = 1 - ssim(frames[i], frames[i-1])    # what "intensity" means
> displacement[i] = 1 - ssim(frames[i], frames[0])   # what the notes compute
> ```
>
> The two differ sharply for a **hold**: during a static hold at the top of a
> stroke, true intensity is ~0 while the notes' curve stays **high**. Since
> holds are exactly the *"pause carried in a movement sequence"* the method is
> introduced to analyse, this is not a quibble.
>
> Both quantities peak at the stroke, so peak detection works either way ? which
> is presumably why the discrepancy went unnoticed.

## Why SSIM rather than pixel difference

SSIM compares **luminance, contrast and structure** rather than raw values, so
it is robust to global brightness changes and to noise. A plain
sum-of-absolute-differences would register a lighting flicker as motion.

> [!note] Yet another similarity measure, and this one *is* normalised
> The module's running similarity measures ? the
> [[neural-similarity-and-dot-product|dot product]] (L02),
> [[cross-correlation-localisation|cross-correlation]] (L05),
> [[convolutional-network|convolution]] (L06) ? all omit normalisation, which
> the wiki has flagged three times. SSIM is built around normalised statistics
> (means, variances, covariance), and is the second normalised comparison in the
> module after L09's [[saliency-model]].

## See also

- [[gesture-phases]] ? [[snapshot-model]] ? [[motion-history-image]]
