---
title: The XOR Problem
type: concept
tags: [foundations, geometry, neural-networks]
sources: [L03]
created: 2026-09-20
updated: 2026-09-20
status: solid
---

# The XOR Problem

**Perceptron: limitations in learning Boolean functions** (L03, p2). A single
perceptron can learn OR and AND. It cannot learn XOR.

## Biological origin

None. This is the classic demonstration that a single unit is a *linear*
classifier, and it is the historical reason multilayer networks exist.

## The demonstration (L03, p2)

The lecture plots each Boolean function in the $(x_1, x_2)$ plane and tries to
draw one line separating the true outputs from the false ones:

| Function | True for | Separable by one line? |
|---|---|---|
| **OR** | $(0,1), (1,0), (1,1)$ | ✅ yes |
| **AND** | $(1,1)$ | ✅ yes |
| **XOR** | $(0,1), (1,0)$ | ❌ **no** |

For XOR the two true points sit on one diagonal and the two false points on the
other. No straight line separates the diagonals — the notes mark this with a
lightning bolt and "not".

This is the consequence [[linear-separability]] set up in L02 but never stated:
one unit draws exactly one hyperplane, so any function whose classes are not
linearly separable is out of reach. Combined with the
[[perceptron-convergence-theorem]], the situation is worse than "inaccurate" —
the learning rule simply never terminates.

## The resolution (L03, p2)

> **Multi-Layer Perceptron: Boolean functions.** Each perceptron in the lower
> layer can separate **linearly**. The output neuron in the last layer can
> separate the input space **non-linearly**. (L03)

Two lines, combined by a third unit, carve out the diagonal band that XOR needs.

```text
# XOR is not linearly separable
#
#   x2
#    1 |  T        F              T = true (XOR = 1)
#      |                          F = false
#    0 |  F        T
#      +-------------- x1
#         0        1
#
# no single straight line puts both T on one side.

# --- resolution: two lines, then combine ---
h1 = step(x1 + x2 - 0.5)        # OR     : true unless both are 0
h2 = step(x1 + x2 - 1.5)        # AND    : true only when both are 1
y  = step(h1 - h2 - 0.5)        # OR AND NOT AND  ==  XOR

# each hidden unit draws ONE line ([[linear-separability]]);
# the output unit combines them into a non-linear region.
# [external] these specific weights are a standard construction; L03 gives the
# argument and the diagrams but not the numbers.
```

## Where it appears in the module

- [[L03-computational-neural-networks]] — page 2.

## See also

- [[linear-separability]] — the L02 page that set this up.
- [[multi-layer-perceptron]] — the resolution.
- [[perceptron-convergence-theorem]] — why failure here is non-termination
  rather than a bad answer.
- [[backpropagation]] — needed because the [[perceptron-learning-rule]] cannot
  train the hidden layer that the resolution requires.

## Open questions / gaps

- The notes give the geometric argument and the diagrams but **no weights** for
  a working XOR network.
- NAND and NOR are not discussed.
- The historical significance (the Minsky–Papert critique and the resulting AI
  winter) is not mentioned — the limitation is presented as a fact, not as an
  episode.
