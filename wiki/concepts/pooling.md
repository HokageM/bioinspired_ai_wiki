---
type: concept
tags: [convolution, architecture, vision]
sources: [L06]
status: solid
---

# Pooling

The dimensionality-reduction step in a [[convolutional-network]], and the model
counterpart of the **complex cell**.

## As given in L06

From [[L06-hierarchical-vision]], p7:

- **`max`** (or **average**) over a window
- Pooling layer `p⃗` with **receptive field `r_p = 2`** and **stride
  `s_p = 2`**
- Worked dimensions: `dim(z⃗) = 8×2` ⇒ **`dim(p⃗) = 4×2`**

```
z_{0,0} z_{1,0} │ z_{0,1} z_{1,1} │ z_{0,2} z_{1,2} │ z_{0,3} z_{1,3}
        └── max ──┘         └── max ──┘   …
              p_{0,0}            p_{1,0}       …
```

The lecture introduces it as **dimensionality reduction** following
*2D convolutional layers with multiple filters*, whose output has dimension
`n_f × r` for `n_f` filters.

## Verified

With the same patch-count arithmetic as convolution —
`N = (|x| − r_p)/s_p + 1` — a non-overlapping window `r_p = s_p = 2` halves each
spatial dimension: `8 → 4`, leaving the filter dimension `2` untouched. So
`8×2 → 4×2`. **The worked example in the notes is correct.**

Note that `r_p = s_p` means windows **tile without overlap**, which is why the
reduction is exactly a factor of 2. Convolution in this lecture uses `s = 1`,
so convolution windows *do* overlap and the size barely shrinks. The two layers
have different jobs: convolution detects, pooling discards.

## What it is actually for

Three reasons, only the first of which the notes give:

1. **Dimensionality reduction** — fewer activations to carry forward, fewer
   parameters downstream.
2. **Translational invariance** — this is the biological one. A max over a
   window returns the same value wherever in the window the feature appeared.
   Position information is deliberately destroyed. `[external]`
3. **Larger effective receptive fields** — after pooling, a kernel of the same
   size `r` covers twice as much of the original image, so deeper layers see
   more context without more weights. `[external]`

Reason 2 is the direct implementation of the **complex cell** described in
[[simple-complex-hypercomplex-cells]]:

> **Complex cells:** orientation detector, **implements invariant features**,
> e.g. **translational invariance**

and of the **C-cells** of the [[neocognitron]]:

> **C-cells inserted to correct for positional errors: receive responses from
> S-cells coding for the same feature.**

*Correcting for positional errors* and *max over a spatial window* are the same
operation. The lecture places these on facing material and does not quite say so
outright, but the vocabulary makes the derivation visible.

## Pseudocode

```
function max_pool_1d(z, r_p, s_p):
    N = (len(z) - r_p) / s_p + 1
    p = []
    for i in 0 .. N-1:
        window = z[ i*s_p : i*s_p + r_p ]
        p.append( max(window) )
    return p

function avg_pool_1d(z, r_p, s_p):
    # identical, with mean(window) instead of max(window)
```

```
# 2D, preserving the filter/channel dimension — this is the 8x2 -> 4x2 case
function max_pool_2d(z, r_p, s_p):        # z[position][filter]
    for f in filters:
        for i in 0 .. (len(z) - r_p)/s_p:
            p[i][f] = max( z[i*s_p + j][f]  for j in 0..r_p-1 )
    return p
```

```
# Why max and not average, informally:
#   max     — "was this feature present anywhere in the window?"   (detection)
#   average — "how much of this feature was in the window?"        (density)
# A complex cell answers the first question.
```

## Unclear in the source

- The notes offer **max or average** with no guidance on when to use which, and
  no reason given for either.
- The **invariance** justification is stated for complex cells on p3 and for
  C-cells on p6 but is never connected to the pooling formalism on p7, even
  though that is the whole point of the operation. The reader is left to join
  three separate pages.
- **No pooling backward pass** is given, though pooling sits in the middle of a
  network trained by [[backpropagation]]. (For max pooling the gradient routes
  entirely to the argmax element; for average pooling it spreads evenly.
  `[external]`)

## Related

[[convolutional-network]] · [[simple-complex-hypercomplex-cells]] ·
[[neocognitron]] · [[receptive-field]] · [[weight-sharing]] ·
[[network-architectures]] · [[L06-hierarchical-vision]]
