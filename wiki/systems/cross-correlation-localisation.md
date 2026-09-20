---
type: system
title: Cross-Correlation for Localisation
sources: [L05, L09]
tags: [audition, localisation, algorithm, signal-processing]
---

# Cross-Correlation for Localisation

The algorithmic half of [[hybrid-acoustic-tracking]]. No training, no
parameters — given two microphone signals it returns the
[[interaural-time-difference]], and from that the angle of incidence.

## What it does

- **Determine maximum similarity between two signals `g(t)` and `h(t)`.**
- **The correlation vector represents the ITD delay between signals.**
- **Allows then to determine the angle of incidence of the source.**

And the two-line definition from the margin:

> **Similarity is the max dot product of shifted 0-padded inputs.**
> **Delay is the time difference at maximal similarity.**

## The algorithm (L05, p3)

**Input:** 2 vectors representing digitally sampled sound signals `g(t)` and
`h(t)`. If the vectors are not the same length, the shorter vector is
**zero-padded** to the length of the larger vector.

Cross-correlation **offsets signal `h(t)` to the left of `g(t)`**.

**Output:** calculates the sum of the product of values of overlapping `g(t)`
and `h(t)` values.

```
1)  Set delay d = |g⃗| and similarity s = 0

2)  For d_i = |g⃗| − 1  to  −(|g⃗| − 1)
      i)   zero-pad g⃗ from right to left, and h⃗ from left to right, by d_i zeros
      ii)  s_i = g⃗_zp ∘ h⃗_zp
      iii) if s_i ≥ s then s = s_i, d = d_i

3)  Return s, d
```

The worked example on the page:

```
g = 1 1 2 3 2 1 1 1        ──── d ────→
h =        1 1 1 1 2 3 2 1  ←──────────
```

## Pseudocode

```
# ---- cross-correlation, as specified in L05 ----
def cross_correlate(g, h):
    n <- max(len(g), len(h))
    g <- zero_pad_to(g, n)                 # shorter vector padded to longer
    h <- zero_pad_to(h, n)

    best_s <- 0
    best_d <- n

    for d_i in range(n-1, -(n-1) - 1, -1):     # from +(n-1) down to -(n-1)
        # slide h against g by d_i samples
        s_i <- 0
        for k in 0 .. n-1:
            if 0 <= k - d_i < n:
                s_i <- s_i + g[k] * h[k - d_i]     # dot product of the overlap

        if s_i >= best_s:
            best_s <- s_i
            best_d <- d_i

    return best_s, best_d

# ---- from delay to angle (see geometric-sound-localisation) ----
def angle_of_incidence(g, h, c, r, b):
    _, d <- cross_correlate(g, h)
    a <- d * c / r                  # path-length difference, metres
    return arccos(a / b)
```

The inner loop is exactly a dot product over the overlapping region — which is
why the marginal note calls similarity *the max dot product of shifted 0-padded
inputs*. Padding is just bookkeeping so that non-overlapping samples contribute
zero.

## Complexity and its consequence

`O(n²)` as written: `2n − 1` shifts, each costing up to `n` multiply-adds. For
real-time robot audition on long buffers this is the bottleneck, which is part
of why the system correlates short windows and hands prediction off to the
[[simple-recurrent-network]] tracker. The notes do not discuss complexity.
FFT-based correlation is `O(n log n)`. `[external]`

## The link back to L02

This is [[neural-similarity-and-dot-product]] with a sweep over shifts. L02
established that a neuron's weighted sum measures the alignment between its
input and its weight vector — a template match. Here one signal is the template
and the other is the input, and the algorithm asks *at which shift do they match
best*.

The biological version of the same idea is the [[jeffress-model]]: a bank of
coincidence detectors, one per delay, is a bank of shifted template matchers
evaluated in parallel. The algorithm loops over `d_i`; the brain lays the `d_i`
out in space and runs them all at once.

## Why it is not learned

*Cross-correlation does not require training to provide azimuth angle.* The
mapping from signals to ITD is fully determined by physics, so there is nothing
to fit. See [[hybrid-architecture]] — this is the lecture's argument for
spending learning capacity only where the problem is genuinely uncertain.

## Unclear in the source

- Step 1 sets `d = |g⃗|`, a value outside the loop's range `[−(n−1), n−1]`. It is
  a sentinel; since `s` starts at 0 and any non-negative correlation triggers
  the update, it is overwritten on the first iteration in practice.
- `s_i ≥ s` (not `>`) means **later** shifts win ties, biasing towards the most
  negative `d_i`. Probably unintended.
- **No normalisation.** The raw dot product favours shifts with more overlap and
  is sensitive to signal amplitude. Normalised cross-correlation divides by the
  product of the norms. `[external]`
- `σ = # of offsets from cc` appears in the p2 block diagram and is never used
  again.

## See also

[[hybrid-acoustic-tracking]] · [[geometric-sound-localisation]] ·
[[interaural-time-difference]] · [[jeffress-model]] ·
[[neural-similarity-and-dot-product]] · [[hybrid-architecture]] ·
[[L05-robot-sound-localisation]]


## What localisation does not give you (L09)

[[auditory-scene-analysis]] in [[L09-bio-inspired-attention]] draws the line
this page needs:

> **Auditory attention: localise sound sources and filter out irrelevant sound
> information.**

Cross-correlation does the first clause. It does **not** do the second — knowing
*where* each talker is does not tell you which acoustic energy belongs to which
talker, nor which one you care about. That is the grouping problem, and it is
what [[auditory-attention-model|ASA]] adds.

> [!success] The cocktail party thread, closed
> ~~L05 poses the cocktail party problem and leaves it unsolved.~~ L09 names the
> solution's shape: group → segregate → compete, with top-down bias at every
> stage. It remains an architecture rather than an algorithm — no grouping cue
> is specified, and **ITD, which this page computes, is the obvious one and is
> not mentioned**.
