---
title: Levels of abstraction in deep networks
type: concept
sources: [L12]
tags: [hierarchy, representation, explainability]
updated: 2026-09-21
---

# Learning hierarchical features

> **Towards explainable deep learning: learning hierarchical features.**
> **Use of trained feature extractors to model data representation in CNN.**
> **Using sequences of layers to obtain hierarchical data representation: from
> low level features to higher level of abstraction.**

```
Input ? 1st layer features ? 2nd layer features ? 3rd layer features ? feature
        (edges)              (parts)              (objects)            vector
```

> **Each layer increases the level of abstraction of the modelled data.**
>
> | Domain | Ladder |
> |---|---|
> | **Image** | pixel ? edges ? shapes ? objects |
> | **Text** | character ? word ? sentence ? story |
> | **Speech** | sound ? phone ? phoneme ? word |

## Why this is offered as an explanation

If each layer corresponds to a nameable level of description, then the network's
internals are **already** interpretable in principle ? you do not need to extract
anything, you need only look at the right layer. That is the argument, and it is
why this page opens the explainable-deep-learning section.

> [!note] The same ladder as L06, now used for a different purpose
> [[L06-hierarchical-vision]] gave *edges ? parts ? objects* as a claim about the
> **visual cortex** ? [[simple-complex-hypercomplex-cells|simple, complex and
> hypercomplex cells]], with a trained CNN's first layer discovering oriented
> edge detectors as supporting evidence.
>
> L12 gives the identical ladder as a claim about **interpretability**. Same
> structure, two arguments: L06 says *therefore it is brain-like*, L12 says
> *therefore it is legible*. Neither lecture cites the other, and they are the
> module's strongest and most reused idea.

> [!warning] The text and speech ladders are not the same kind of claim
> *Pixel ? edges ? shapes ? objects* is supported: trained vision networks
> demonstrably develop these features, and L06 shows the first rung directly.
>
> *Character ? word ? sentence ? story* and *sound ? phone ? phoneme ? word* are
> descriptions of the **domain's** structure, not findings about any network. No
> evidence is offered that a text model's third layer represents sentences.
> Presenting all three in one table implies a uniformity that only the first
> entry has earned.
>
> The speech ladder is also mis-ordered: a **phoneme** is the abstract category
> and a **phone** its concrete realisation, so *sound ? phone ? phoneme ? word*
> is right only if phone precedes phoneme, which inverts the usual usage
> [external] ? probably a slip.

## What the argument leaves out

Layer `k` being *"more abstract"* than layer `k?1` does not make it **nameable**.
A [[local-vs-distributed-representation|distributed]] layer can encode object
identity perfectly while no single unit corresponds to any object ? which is
precisely L02's point, and precisely why [[class-activation-map]] and
[[layer-wise-relevance-propagation]] are needed two pages later.

Abstraction and interpretability are different properties, and the lecture treats
the first as evidence for the second.

## See also

- [[L06-hierarchical-vision]] ? [[convolutional-network]] ?
  [[class-activation-map]] ? [[explainable-ai]]
