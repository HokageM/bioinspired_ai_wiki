---
title: David Hubel
type: entity
tags: [people, neuroscience, vision]
sources: [L06]
created: 2026-09-21
updated: 2026-09-21
status: stub-by-source
---

# David Hubel

Named in [[L06-hierarchical-vision]] p4 as one half of "**Hubel and Wiesel**",
alongside [[torsten-wiesel]].

## What the source says

The notes give the finding, not the person:

> Cells in the **striate cortex** respond best to **bars of light** rather than
> to spots of light.
> - simple cells: bars of light **/** bars of dark
> - complex cells: bars of light **&** bars of dark

and **[[orientation-tuning]]** — *the tendency of neurons in the striate cortex
to respond more to bars of certain orientations and less to others*, with a
response rate that falls off with angular difference from the preferred
orientation.

No first name, no dates, no institution, no publication.

## Why the finding matters to the module

It is the empirical foundation of the whole lecture. The
simple/complex/hypercomplex ladder of
[[simple-complex-hypercomplex-cells]] is what [[kunihiko-fukushima]] turned
into the [[neocognitron]]'s S-cells and C-cells, which [[yann-lecun]] turned
into the [[convolutional-network]]'s convolution and [[pooling]] layers.

Every deep vision model in use today traces back through that chain to a
microelectrode in a cat's striate cortex. This is the module's single
best-documented case of biology actually driving architecture, rather than
being invoked after the fact — see [[ann-brain-correspondence]].

The *bars not spots* detail is the crux: it means V1 is not simply inheriting
the centre-surround [[receptive-field]] of [[the-retina]] but **composing**
several of them into something new. That is the first step of the hierarchy.

## Related

[[torsten-wiesel]] · [[simple-complex-hypercomplex-cells]] ·
[[orientation-tuning]] · [[receptive-field]] · [[visual-pathway]] ·
[[neocognitron]] · [[convolutional-network]] · [[L06-hierarchical-vision]]
