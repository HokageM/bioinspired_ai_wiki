---
title: Yann LeCun
type: entity
tags: [people, neural-networks, vision]
sources: [L06]
created: 2026-09-21
updated: 2026-09-21
status: stub-by-source
---

# Yann LeCun

Credited in [[L06-hierarchical-vision]] p6 with the **Convolutional NN**, and
named again in the closing summary alongside [[kunihiko-fukushima]] under
*object recognition*.

## What the source says

The attribution is a parenthetical — "Convolutional NN (LeCun)" — accompanied by
the **LeNet-5** architecture diagram:

```
32×32 → 6@28×28 → 6@14×14 → 16@10×10 → 16@5×5 → 120 → 84 → 10
      conv     subsample    conv     subsample  full  Gaussian
```

No first name, date, institution, or publication. The name **LeNet** itself is
not written in the notes — only the diagram, which is recognisably LeNet-5.
`[external]`

## Position in the chain

The final link:

```
Hubel & Wiesel  →  Fukushima  →  LeCun
  measurement       structure     learning
```

[[kunihiko-fukushima]] had the architecture but could only train part of it.
LeCun's contribution, in the terms this module cares about, is that the
whole hierarchy became **trainable end-to-end by [[backpropagation]]** —
including the feature detectors, which the [[neocognitron]] had to train
separately and unsupervised.

The irony the lecture then develops: this is achieved through
[[weight-sharing]], and *weight sharing is the one part of the design that
neurons cannot implement.* Making the model trainable made it less biological.
That tension is the subject of [[dynamic-weight-sharing]] and the sharpest
argument in the module — see [[ann-brain-correspondence]].

## Related

[[convolutional-network]] · [[neocognitron]] · [[kunihiko-fukushima]] ·
[[weight-sharing]] · [[pooling]] · [[backpropagation]] ·
[[L06-hierarchical-vision]] · [[L03-computational-neural-networks]]
