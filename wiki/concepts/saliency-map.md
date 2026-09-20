---
title: Saliency map
type: concept
sources: [L09]
tags: [attention, vision, representation, coding]
updated: 2026-09-21
---

# Saliency map

A single topographic map in which **activity encodes how much a location
deserves attention**, regardless of which feature made it interesting.

> **The saliency map awards higher responses to more salient image locations.**

## A saliency map in primary cortex

> - **Those responses are those of V1 cells tuned to input features** (colour, etc.)
> - **Firing rates of V1's output neurons increase monotonically with the
>   salience value of the visual input.**

The claim is strong and specific: V1 is not *only* a feature detector; its output
firing rate already carries **saliency**, and a downstream reader needs no
feature identity at all ? it can simply take the maximum.

The notes' illustration:

```
In:   = = = = | | |          In:   - - - | - -
Out:  . . . . ? . .          Out:  - - - ? - -
```

Uniform regions are suppressed; the discrepant element survives. This is
**centre?surround, applied to features rather than to luminance** ? and
centre?surround on luminance is what [[the-retina]] does in L06. The same
operation, one level up and one feature-space over.

> [!note] Saliency is the sixth thing V1 has been asked to do
> [[L06-hierarchical-vision]] gave V1 [[simple-complex-hypercomplex-cells|simple,
> complex and hypercomplex cells]], [[orientation-tuning]], and the first stage
> of the [[two-visual-streams]]. L09 adds a saliency map. The module never
> discusses how one population carries a feature code *and* a priority code
> simultaneously ? though a **monotonic** firing-rate relationship to salience
> is in direct tension with tuning, since a cell's rate is then ambiguous between
> *my preferred feature is present* and *whatever is here is unusual*.

## Why one map and not many

Attention has to produce **one** decision. Feature maps disagree ? the reddest
thing and the most obliquely oriented thing are different locations ? so
selection needs a common currency. The saliency map is that currency:
feature-blind, purely topographic, and therefore directly readable by
[[winner-take-all]].

This is the same architectural need as L07's [[superior-colliculus]], where
several modality maps in register feed one orienting decision. Two lectures, two
maps, same job: **convert heterogeneous evidence into one comparable surface,
then take the maximum.**

## See also

- [[saliency-model]] ? [[pop-out-effect]] ? [[winner-take-all]]
- [[place-cells]] ? the module's running topographic-code pattern
