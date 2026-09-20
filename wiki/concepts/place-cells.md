---
title: Place Cells
type: concept
tags: [neuroscience, navigation]
sources: [L01, L06, L07]
created: 2026-09-20
updated: 2026-09-21
status: developing
---

# Place Cells

Neurons that form an **inner map of the environment**: each cell is active in a
particular place (L01).

## Biological origin

A neural mechanism observed in animals, used in
[[L01-introduction-to-bio-inspired-ai]] as the example of a biological solution
worth copying. The notes give no species, brain region, or discoverer.

## Computational form

The lecture gives no model. The behaviour as described — one unit responding to
one location — is structurally identical to the *local* scheme in
[[local-vs-distributed-representation]]: one neuron stands for one item.

```text
# place-cell readout as described in L01 (one cell ≈ one place)
for cell in place_cells:
    cell.active = (agent_position is within cell.place_field)

# the "inner map" is then just which cell is on
estimated_place = argmax(cell.active for cell in place_cells)
# [external] real place fields are graded and overlapping, not binary;
# L01 does not say which.
```

## Where it appears in the module

- [[L01-introduction-to-bio-inspired-ai]] — the navigation example.

## See also

- [[grid-cells]] — the complementary mechanism; combined with place cells they
  give a *comprehensive inner positioning system* (L01).
- [[local-vs-distributed-representation]] — the same one-unit-one-thing coding
  question, treated computationally in L02.

## Open questions / gaps

- Not revisited in [[L02-spiking-neural-networks]] or modelled anywhere yet.
- The notes do not say how place cells and grid cells interact to produce the
  combined positioning system — only that they do.


## The pattern recurs in vision (L06)

[[L06-hierarchical-vision]] adds a fifth instance of the representational trick
this page introduced. V1 cells have a **preferred edge orientation** with a
graded falloff either side of it ([[orientation-tuning]]), so orientation is
encoded by *which cell in an ordered bank* responds most ? exactly as position
is encoded by which place cell fires.

| Lecture | Ordered population | Encodes |
|---|---|---|
| L01 | **place cells** | position in space |
| L04 | [[self-organising-map]] units | position in feature space |
| L05 | [[tonotopic-representation]] | frequency |
| L05 | [[jeffress-model]] detectors | interaural delay ? angle |
| **L06** | **orientation-tuned V1 cells** | **edge angle** |

Five lectures, five instances, and the module has never named the pattern. See
[[overview]] and [[local-vs-distributed-representation]].


## Sixth instance: the SC's columns (L07)

[[L07-crossmodal-processing]] supplies another:

> **SC is topographically organised: depending on where a stimulus is, it
> generates activity in a specific column of the SC.**

What is new is that the [[superior-colliculus]] stacks **several** such maps ?
one per modality ? **in register**, so a single column means the same position in
the visual layer, the auditory layer and the somatosensory layer. The
representational trick is not merely repeated here; it is repeated *and aligned*,
and that alignment is what makes [[multisensory-integration]] possible without
any explicit coordinate conversion.

Sixth instance across seven lectures. Still unnamed by the module.
