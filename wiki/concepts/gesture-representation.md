---
title: Gesture representation
type: concept
sources: [L10]
tags: [gesture, vision, representation]
updated: 2026-09-21
---

# Gesture representation

> **Appearance-based:** hand contours ? image moments ? optical flow
> **Model-based:** joints to model finger movements ? mesh grid ? geometric hand
> model

## The distinction

**Appearance-based** works on **what the camera sees**. No assumption about what
a hand is; the representation is a function of pixels.

**Model-based** fits an **explicit parametric hand** ? joints, a mesh, a
geometric solid ? and represents the gesture by its parameters.

| | Appearance | Model |
|---|---|---|
| Assumes | nothing about hands | a full hand model |
| Output | image-derived features | joint angles / pose |
| Fails when | viewpoint or lighting changes | fitting fails |
| Cost | cheap | expensive |
| Invariance | must be learned | built in |

> [!note] This is the module's modular-versus-end-to-end fork again
> A model-based representation is [[functional-decomposition]]: fit a model, then
> reason over it, with the [[functional-decomposition|granularity problem]]
> included free ? how many joints is enough? An appearance-based representation
> declines to build the model.
>
> [[object-picking-architecture]] (L08) drew the same axis for grasping;
> [[imitation-learning]] (L08) took the model side. The module reaches this fork
> in four separate lectures and names it differently each time.

## Where the lecture ends up

**Both**, and the interesting part is *why*. The deep-network critique in L10 is
that DNNs are **data-hungry** and **computationally demanding**, so the answer is
to move work out of the network:

> **Benefit from new preprocessing technology: skeletal data provides necessary
> body joints.**

[[openpose]] produces a model-based representation *with a neural network*, and
everything downstream then operates on **joints instead of pixels** ? far lower
dimensional, and already invariant to appearance.

So the fork dissolves: learn the model, then reason over it. That is a genuinely
different position from either pole, and the lecture does not name it as such.

## Optical flow

Listed under appearance-based. Note that it is the only appearance feature that
is **temporal** ? contours and image moments describe a single frame. Its
natural competitors in this lecture are [[motion-history-image|MHI]] and the
differential image sequence used by [[cnn-lstm]], both of which are ways of
getting motion into a static-looking input so that a
[[convolutional-network|CNN]] can process it.

## See also

- [[static-and-dynamic-gestures]] ? [[openpose]] ? [[human-pose-estimation]] ?
  [[motion-history-image]]
