---
title: Saliency model
type: system
sources: [L09]
tags: [attention, vision, architecture]
updated: 2026-09-21
---

# Saliency model

> **Model "pop-out" effect and saliency processing (*Auspr?gung*).**
>
> **Input ? Feature Map ? Saliency Map ? Central Representation**
> **\* Winner-Take-All is the core algorithm**

with the marginal note: **centre?surround differences and normalise**, and the
feature list *colour, orientation, position*.

## The pipeline

| Stage | Does |
|---|---|
| **Input** | the image |
| **Feature maps** | one map per feature ? colour, orientation, position |
| | *centre?surround differences, then normalise* |
| **Saliency map** | one feature-blind topographic priority map |
| **Central representation** | what is attended, passed on |
| **WTA** | selects the maximum |

## Pseudocode

```
def saliency_model(image, features=(COLOUR, ORIENTATION, POSITION)):
    maps = []
    for f in features:
        m = feature_map(image, f)
        m = centre_surround(m)     # difference from the local neighbourhood
        m = normalise(m)           # so no feature dominates by scale alone
        maps.append(m)
    S = combine(maps)              # <- not specified in the source
    while True:
        loc = argmax(S)            # winner-take-all
        yield central_representation(loc, maps)
        S = inhibit_return(S, loc) # otherwise it attends the same place forever
```

## Why each step is necessary

**Centre?surround** makes saliency *relative*. Absolute feature values cannot
express pop-out, because [[pop-out-effect|pop-out is a property of context]].

**Normalise** makes feature maps **commensurable**. Orientation contrast and
colour contrast have different units and different ranges; without normalisation
whichever feature happens to have the larger numeric scale would always win the
argmax. This is the same problem ? and the same fix ? as
[[histogram-based-som|divisive normalisation]] in L07, where it converts
likelihoods into a comparable population code.

> [!note] Normalisation, finally
> The wiki has repeatedly flagged the module for using the **dot product as
> similarity without normalising** ? L02's neuron, L05's
> [[cross-correlation-localisation|cross-correlation]], L06's convolution. Here
> normalisation is explicit and load-bearing, and is the only place in the module
> where it is treated as necessary rather than optional.

**Inhibition of return** is not in the notes but is forced by the architecture:
WTA is idempotent, so without suppressing the winner the model attends to the
same pixel forever. Flagged as an inference, not a source claim.

## What the source does not give

> [!warning] `combine` is unspecified
> How several normalised feature maps become one saliency map ? sum? max?
> learned weights? ? is the single most consequential free choice in the model,
> and the notes give only the arrow.

- No equations for centre?surround or normalise.
- No evaluation, dataset or comparison.
- No attribution.

## Relation to the rest of the module

This is a **feed-forward hierarchy with a competitive read-out**: exactly
[[convolutional-network|convolution]] (local differencing, shared across the
image) followed by [[pooling|max]] over the whole map. The lecture's own
take-home message says *"CNNs are inspired by the hierarchical process of visual
input on human's occipital lobe"* and does not notice that the saliency model on
the previous page is one.

```
centre_surround  ~  a difference-of-Gaussians convolution kernel
normalise        ~  divisive / response normalisation
argmax over S    ~  global max pooling
```

## See also

- [[saliency-map]] ? [[winner-take-all]] ? [[pop-out-effect]] ?
  [[feature-integration-theory]] ? [[convolutional-network]]
