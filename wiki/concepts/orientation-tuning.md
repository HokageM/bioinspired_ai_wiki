---
type: concept
tags: [vision, neuroscience, neural-coding]
sources: [L06, L09]
status: solid
---

# Orientation tuning

> **The tendency of neurons in the striate cortex to respond more to bars of
> certain orientations and less to others.**
> — [[L06-hierarchical-vision]], p4

**Response rate falls off with the angular difference of the bar from the
preferred orientation.** The notes plot this as a curve peaking at 0° and
decaying to baseline by roughly ±90°.

## The shape of the code

Each cell has a **preferred orientation** and a graded, roughly bell-shaped
response around it. No single cell reports the orientation of an edge — a cell
firing at half its maximum is ambiguous between +30° and −30°. Orientation is
only recoverable from the **population**: whichever cell fires hardest, and how
the activity is distributed around it.

This is the module's recurring representational trick appearing for the fifth
time. See the table on [[overview]]:

| Lecture | Ordered population | Encodes |
|---|---|---|
| L01 | [[place-cells]] | position in space |
| L04 | [[self-organising-map]] units | position in feature space |
| L05 | [[tonotopic-representation]] | frequency |
| L05 | [[jeffress-model]] coincidence detectors | interaural delay ⇒ angle |
| **L06** | **orientation-tuned V1 cells** | **edge angle** |

A continuous quantity is represented by *which* unit in an ordered bank responds
most, with neighbouring units responding partially. Five instances across five
lectures and **the module has still never named the pattern**. See
[[neural-coding]] and [[local-vs-distributed-representation]].

## Pseudocode

```
# A bank of orientation-tuned cells covering 0..180°
function orientation_column(image, x, y, n_cells, sigma):
    preferred = [ i * 180 / n_cells  for i in 0..n_cells-1 ]
    responses = []
    for theta_pref in preferred:
        r = 0
        for theta_present in edges_at(image, x, y):
            d = angular_difference(theta_present, theta_pref)   # wrap at 180°
            r += strength(theta_present) * exp(-(d^2) / (2*sigma^2))
        responses.append(r)
    return responses
```

```
# Reading the population back out — the "population vector" decode
function decode_orientation(responses, preferred):
    # weighted circular mean; doubling the angle handles the 180° wrap
    sx = sum( r * sin(2*theta) for r,theta in zip(responses,preferred) )
    sy = sum( r * cos(2*theta) for r,theta in zip(responses,preferred) )
    return atan2(sx, sy) / 2
```

The decode step is `[external]` — the notes plot the tuning curve but never ask
how downstream areas read it. It is included because the same question was left
open for [[tonotopic-representation]] and the [[jeffress-model]], and the answer
is the same each time.

## Why tuning width matters

Narrow tuning gives precision but leaves gaps between cells; broad tuning gives
coverage but poor discrimination. The notes' curve is broad — nonzero across
most of ±90° — which means many cells respond to any given edge, and the
information lives in the *pattern*, not the winner.

This directly prefigures the trade discussed in
[[simple-complex-hypercomplex-cells]]: simple cells are *highly selective*,
complex cells trade selectivity for invariance.

## Unclear in the source

- The **width** of the tuning curve is drawn but never quantified.
- The notes do not say whether a cell's preferred orientation is **innate or
  learned**. This matters for the module's argument — if it is learned, it is
  evidence for the unsupervised feature extraction that the
  [[neocognitron]]'s S-cells perform; if innate, it is not.
- Orientation is assigned to **V1 cells** here but to **V4** in the visual-areas
  list on p1. See the warning on [[visual-pathway]].

## Related

[[simple-complex-hypercomplex-cells]] · [[receptive-field]] · [[david-hubel]] ·
[[torsten-wiesel]] · [[neural-coding]] · [[place-cells]] ·
[[tonotopic-representation]] · [[local-vs-distributed-representation]] ·
[[L06-hierarchical-vision]]


## V1 as a saliency map (L09)

[[L09-bio-inspired-attention]] adds a claim about the same cells:

> The saliency map's responses **are those of V1 cells tuned to input features**,
> and **firing rates of V1's output neurons increase monotonically with the
> salience value of the visual input.**

> [!warning] Monotonic-in-salience is in tension with tuning
> A tuned cell's rate says *how close the stimulus is to my preferred
> orientation*. A salience-monotonic rate says *how unusual this location is*.
> A single rate cannot unambiguously carry both: a high rate would be
> indeterminate between *my feature is present* and *something odd is here*.
>
> The standard resolution [external] is that tuning sets **which** cells respond
> and contextual (surround) modulation sets **how strongly** — so identity is
> in the population and salience in the amplitude. The lecture asserts the
> monotonic relation without addressing the conflict, and L06 never mentions
> surround modulation at all.

See [[saliency-map]].
