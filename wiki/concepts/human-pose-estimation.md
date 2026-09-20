---
title: Human pose estimation (HPE)
type: concept
sources: [L10]
tags: [vision, gesture, representation, methods]
updated: 2026-09-21
---

# Human pose estimation

> **Benefit from new preprocessing technology: skeletal data provides necessary
> body joints.**

Recovering the **positions of body joints** from an image, so that downstream
models work on a skeleton rather than on pixels.

## The taxonomy given

| Axis | Options |
|---|---|
| Dimensionality | **2D** or **3D** |
| Subjects | **single-person** or **multi-person** |
| Method | **regression** (predict coordinates directly) or **body-part detection** (find parts, then assemble) |
| Order | **top-down** (detect people, then find joints in each) or **bottom-up** (detect all joints, then group into people) |

[[openpose]] is **bottom-up**.

## Why the bottom-up/top-down axis is the interesting one

**Top-down** cost scales with the **number of people** ? run a joint detector per
detected person. **Bottom-up** cost is constant: detect every joint in the image
once, then solve an assignment problem. That is why OpenPose is real-time for
crowds and top-down methods are not.

> [!note] A fourth appearance of top-down vs bottom-up, and it means something else again
> | Lecture | Sense |
> |---|---|
> | [[visual-pathway]] (L06) | anatomical direction of signal flow |
> | [[top-down-modulation]] (L07?L09) | goals biasing perception |
> | [[subsumption-architecture|subsumption]] (L08) | design methodology |
> | **HPE (L10)** | **order of inference: parts-then-wholes, or wholes-then-parts** |
>
> Here it is closest to the L06 sense ? assemble small evidence into large
> structures ? but it is about *algorithmic order*, not anatomy. The module never
> disambiguates the phrase.

## Why it helps the data problem

[[deep-network-tradeoffs]]: deep networks are data-hungry. A skeleton is perhaps
25 ? 2 numbers where an image is 10? pixels, and it is **already invariant** to
clothing, lighting and skin tone. A downstream [[gamma-gwr]] over joints needs
far less data than one over pixels.

The cost is honest to state: the invariance has not been removed, only **moved**
? OpenPose is itself a large supervised network trained on a large annotated
dataset. The data was paid for once, by someone else. The source does not make
this point.

> [!note] It is also [[gesture-representation|model-based representation]]
> ? obtained by a method that is entirely appearance-based. The lecture presents
> the appearance/model dichotomy on page 2 and dissolves it on page 10 without
> noticing.

## See also

- [[openpose]] ? [[gesture-representation]] ? [[gamma-gwr]]
