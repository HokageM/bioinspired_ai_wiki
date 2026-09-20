---
title: Kunihiko Fukushima
type: entity
tags: [people, neural-networks, vision]
sources: [L06]
created: 2026-09-21
updated: 2026-09-21
status: stub-by-source
---

# Kunihiko Fukushima

Credited in [[L06-hierarchical-vision]] p6 with the **[[neocognitron]]**, and
named again in the lecture's closing summary alongside [[yann-lecun]] under
*object recognition*.

## What the source says

The attribution is a parenthetical — "Neocognitron (Fukushima)" — with no first
name, date, institution or publication. What the notes credit him with:

- A **hierarchical multilayer NN for visual recognition**.
- **Alternate planes of simple S-cells (feature extraction) and complex C-cells
  (positional errors)**, which **resemble processing stages in the visual
  cortex**.
- **S-cells trained with unsupervised or supervised methods; only S-cells have
  learning inputs.**

## Position in the chain

Fukushima occupies the middle link of the module's one genuinely well-evidenced
biology→architecture derivation:

```
Hubel & Wiesel     →     Fukushima      →      LeCun
measured simple        implemented them        made them trainable
and complex cells      as S-cells and          end-to-end with
in striate cortex      C-cells                 backpropagation
```

The naming is the evidence. Fukushima did not merely note a resemblance — he
borrowed [[david-hubel]] and [[torsten-wiesel]]'s vocabulary wholesale, and the
`S`/`C` in `U_S1`, `U_C1` stand for *simple* and *complex*. Compare
[[ann-brain-correspondence]], where most of the module's claimed
correspondences have no such traceable lineage.

## What the notes omit

`[external]` — none of this is in the source, and it matters for reading the
design:

The Neocognitron dates from **1980**, which is before [[backpropagation]] was
widely established. That is why its C-cells are hard-wired rather than learned,
and why its S-cells needed unsupervised training. The architecture was not a
stylistic choice — it was what could be trained at the time. Without this
context the notes' remark that *only S-cells have learning inputs* reads as
arbitrary.

## Related

[[neocognitron]] · [[convolutional-network]] · [[yann-lecun]] ·
[[david-hubel]] · [[torsten-wiesel]] · [[simple-complex-hypercomplex-cells]] ·
[[pooling]] · [[learning-paradigms]] · [[L06-hierarchical-vision]]
