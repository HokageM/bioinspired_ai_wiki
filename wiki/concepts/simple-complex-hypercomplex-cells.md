---
type: concept
tags: [vision, neuroscience, hierarchy]
sources: [L06]
status: solid
---

# Simple, complex and hypercomplex cells

The three cell types found in **V1** (striate cortex), per
[[L06-hierarchical-vision]]. They form a ladder of increasing abstraction and
increasing invariance, and that ladder is the direct ancestor of every deep
vision architecture.

| Cell type | Detects | Property |
|---|---|---|
| **Simple** | line / edge | **highly selective** |
| **Complex** | orientation | **implements invariant features** (e.g. translational invariance) |
| **Hypercomplex** | angle / length | **composed of several complex cells** |

## The trade drawn in the notes

Selectivity and invariance pull against each other, and the hierarchy resolves
the tension by stacking:

- a **simple cell** fires for a bar at one orientation *at one position*. Very
  informative when it fires, but silent if the bar shifts.
- a **complex cell** pools several simple cells with the same preferred
  orientation but different positions. It fires for that orientation
  *anywhere in its field* — it has traded position information for robustness.
- a **hypercomplex cell** is **composed of several complex cells** and so can
  signal a conjunction: an angle (two orientations meeting) or a length (an
  orientation that stops).

## [[david-hubel]] and [[torsten-wiesel]]'s finding

Cells in the striate cortex respond best to **bars of light** rather than to
**spots** of light. The distinction between the first two types is given in the
notes as:

- **simple cells:** bars of light **/** bars of dark (one *or* the other)
- **complex cells:** bars of light **&** bars of dark (either)

Simple cell processing is drawn two ways: **a) edge detector**, **b) stripe
detector** — differing in whether the excitatory subregion sits beside one
inhibitory region or between two.

## Pseudocode

```
# Simple cell: oriented, position-specific. A weighted sum over its RF.
function simple_cell(image, x, y, theta):
    k = oriented_bar_kernel(theta)        # + along the bar, − beside it
    return rectify( sum(k * patch(image, x, y)) )
```

```
# Complex cell: same orientation, pooled over position ⇒ translation invariance
function complex_cell(image, x, y, theta, positions):
    return max( simple_cell(image, x+dx, y+dy, theta)
                for (dx,dy) in positions )
```

```
# Hypercomplex cell: a conjunction of complex cells ⇒ angles, line endings
function hypercomplex_cell(image, x, y, theta1, theta2):
    a = complex_cell(image, x, y, theta1)
    b = complex_cell(image, x, y, theta2)
    end_stop = complex_cell(image, x + beyond, y, theta1)
    return rectify( a + b - end_stop )   # fires for a corner, not a long line
```

Note that `complex_cell` is a **max over positions**. That is exactly max
[[pooling]]. The correspondence is not analogical — it is the same operation.

## Where this goes

| L06 biology | L06 model |
|---|---|
| simple cell | convolution with a learned filter |
| complex cell | **[[pooling]]** |
| hypercomplex cell | a deeper convolution over pooled maps |

[[kunihiko-fukushima]] built this literally: the [[neocognitron]] alternates
**S-cells** (feature extraction — simple) with **C-cells** (positional error
correction — complex). [[yann-lecun]]'s [[convolutional-network]] inherits it as
alternating convolution and subsampling layers.

> [!note] A correspondence that survives scrutiny
> Most of the module's brain↔network claims are asserted rather than
> demonstrated — see [[ann-brain-correspondence]]. This one has a documented
> chain of derivation: Hubel & Wiesel measured it, Fukushima implemented it,
> LeCun trained it. Each step cites the last.

## Unclear in the source

- The notes do not say **how** complex cells achieve invariance — whether by
  pooling simple cells (the standard hierarchical account) or by some other
  mechanism. The pooling reading is inferred here from the parallel with C-cells
  on p6, which the notes do draw explicitly.
- **Hypercomplex cells** are given as "angle/length detector" with no worked
  example, and play no role in the models that follow.

## Related

[[receptive-field]] · [[orientation-tuning]] · [[david-hubel]] ·
[[torsten-wiesel]] · [[neocognitron]] · [[convolutional-network]] ·
[[pooling]] · [[visual-pathway]] · [[levels-of-abstraction]] ·
[[L06-hierarchical-vision]]
