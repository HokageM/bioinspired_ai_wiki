---
title: Class Activation Map (CAM)
type: system
sources: [L12]
tags: [explainability, vision, convolution, methods]
updated: 2026-09-21
---

# Class Activation Map (CAM)

> **How can we explain what a network "knows"?**
> **E.g. which area was used to classify an image.**
>
> **Class Activation Map (CAM):**
> - **visualize relevant input features** (what pixels were used to classify)
> - **regarding a specific class**
> - **focuses on spatial localization of important regions in the input image for
>   a specific class**

A heatmap over the input, per class.

```
def cam(net, image, target_class):
    A = net.last_conv_feature_maps(image)        # k maps, each H x W
    w = net.classifier_weights[target_class]     # one weight per map
    return sum(w[k] * A[k] for k in range(len(A)))   # weighted sum -> heatmap
```

The construction exploits a property of
[[convolutional-network|convolutional]] layers the module established in L06:
**feature maps preserve spatial layout**. A unit's position in the map still
corresponds to a position in the image, so weighting the maps by their
contribution to a class and summing gives a map of *where that class's evidence
was found*.

> [!note] It works because of weight sharing, and only for convolutional networks
> [[weight-sharing]] is what makes a feature map a **map**. A fully connected
> layer destroys the correspondence between unit and location, and CAM is
> undefined there. The technique is not general-purpose; it is a cash-in on a
> specific architectural property, and the lecture presents it without the
> precondition.

## Per class, which is the important qualifier

The same image produces **different** maps for different classes: asked about
*house* it highlights the building, asked about *person* it highlights the
figure. That is what makes it an explanation of a **decision** rather than a
saliency measure of the image.

> [!note] Not the same as L09's saliency map, despite the resemblance
> | | [[saliency-map]] (L09) | CAM (L12) |
> |---|---|---|
> | Computed from | the **image** | the image **and a class** |
> | Answers | where is this image conspicuous? | where is the evidence for *this* answer? |
> | Direction | bottom-up, stimulus-driven | **top-down**, from the output backwards |
> | Purpose | model attention | explain a model |
>
> Both are 2-D maps of importance over an image, and they are computed in
> opposite directions for unrelated reasons. Another instance of the module using
> one shape of representation for many jobs ? and another instance of two
> lectures not noticing they share a diagram.

## What it can and cannot establish

**Can:** rule out that the network keyed on something irrelevant ? a watermark, a
background, a rule-of-thumb correlate. This is the genuine practical value.

**Cannot:** tell you what the network concluded from the region it used. A map
centred on the correct object is consistent with the network having learned the
concept and with its having learned a texture that happens to co-occur.
Attribution is not mechanism ? see [[explainable-ai]].

## The RL aside

> **Explaining the internals of RL: visualization of critic maps during RL;
> observe stability of maps over time.**

The same idea applied to a reinforcement learner, and **reinforcement learning's
thirteenth appearance by name with no algorithm**. *Critic* is a technical term
naming half of an actor-critic architecture, used here without definition. Watching
a map **stabilise over training** is a genuinely different diagnostic ? a
statement about convergence, not about a decision ? and gets one line.

## See also

- [[layer-wise-relevance-propagation]] ? [[explainable-ai]] ?
  [[convolutional-network]] ? [[saliency-map]]
