---
title: Convolutional Network
type: system
tags: [neural-networks, vision, convolution]
sources: [L03, L06, L10, L12]
created: 2026-09-20
updated: 2026-09-21
status: solid
---

# Convolutional Network

> [!success] Stub resolved at L06
> This page sat as a stub for three lectures. Convolutional networks were named
> **once**, in L03's closing list of "Structures", with no architecture and no
> equations, and the page recorded a lint candidate: *check whether a later
> lecture develops convolutional networks.*
>
> [[L06-hierarchical-vision]] develops them completely — and does so by deriving
> them from the visual cortex, which is exactly the bio-inspired justification
> the L03 stub noted was missing.

Attributed in the notes to **[[yann-lecun]]**, following
[[kunihiko-fukushima]]'s [[neocognitron]].

## Architecture — LeNet-5

The diagram given on L06 p6:

```
32×32 input
   → 6 @ 28×28      convolution
   → 6 @ 14×14      subsampling
   → 16 @ 10×10     convolution
   → 16 @ 5×5       subsampling
   → 120            full connection
   → 84             full connection
   → 10             Gaussian connections (output)
```

**Verified:** the spatial arithmetic is correct throughout. A 5×5 kernel with
stride 1 and no padding takes 32→28 and 14→10; 2×2 subsampling takes 28→14 and
10→5. The channel counts 6, 6, 16, 16 are consistent.

The shape of the network is the shape of the argument: **alternating detection
and reduction**, twice, then a dense classifier. Detection layers find features;
reduction layers throw away where those features were.

## The convolutional layer, formalised (1D)

```
z_i = Σ_{j=0}^{r−1}  w_j · x_{i+j},      i ∈ [0, N]
```

| Symbol | Meaning |
|---|---|
| `r` | **receptive field size** — the filter length (`r = 3` in the example) |
| `s` | **stride** — *how much you jump* (`s = 1` in the example) |
| `N` | **number of patches**, `N = (|x| − r)/s + 1` |
| `p` | **zero padding** — elements added to the input so that `|x| = |z|` |

With `|x| = 8, r = 3, s = 1`: `N = (8−3)/1 + 1 = 6`, matching the `6` on the
diagram. ✅

**With multiple filters**, each filter produces its own **activation map**:

```
z_{i,k} = Σ_{j=0}^{r−1}  w_{j,k} · x_{i+j}
```

*the activation of the i-th neuron on the k-th activation map.* For `n_f`
filters in 2D, the output has dimension `n_f × r`, reduced by [[pooling]].

> [!warning] Off-by-one in the source
> The notes write `i ∈ [0, N]`, but `N` is the *number* of patches, so the
> index should run `[0, N−1]`. With `N = 6` there are patches 0..5.

## Kernels as filters

`eg:` a **horizontal line filter**:

```
−1 −1 −1
 2  2  2
−1 −1 −1
```

Convolved with an image ⇒ **convolved feature / activation map**.

**Verified:** the weights sum to zero, so uniform regions give zero response —
the filter reports *contrast*, not *brightness*. It responds maximally to a
bright horizontal bar one pixel tall with dark above and below.

This is precisely the centre-surround arrangement of a retinal
[[receptive-field]], rotated into a line. Compare [[the-retina]], where
`centre − surround` also sums to zero and also discards absolute illumination.

## Convolution is a sliding dot product

```
z_i = Σ_j w_j · x_{i+j}  =  w · x[i : i+r]
```

Each output is the **dot product** of the filter with a patch — a similarity
measurement. The activation map answers, at every position: *how much does the
image here look like this filter?*

This is the **third** appearance of the dot-product-as-similarity idea in the
module, after L02's neuron and L05's [[cross-correlation-localisation]]. See
[[neural-similarity-and-dot-product]]. Indeed the convolution formula above and
L05's cross-correlation are the **same expression** with a different name for
the shift variable — one slides over space, the other over time delay.

## Pseudocode

```
function conv1d(x, w, s, p):
    x = zero_pad(x, p)
    r = len(w)
    N = (len(x) - r) / s + 1
    z = []
    for i in 0 .. N-1:
        acc = 0
        for j in 0 .. r-1:
            acc += w[j] * x[i*s + j]
        z.append(acc)
    return z                       # len(z) == N
```

```
function conv_layer(x, filters, s, p, phi):
    # filters: n_f kernels; output is n_f activation maps
    return [ [ phi(v) for v in conv1d(x, w_k, s, p) ] for w_k in filters ]
```

```
function lenet5_forward(image):                     # 32x32
    c1 = conv_layer(image, filters=6,  r=5, s=1)    # 6 @ 28x28
    s2 = max_pool(c1, r_p=2, s_p=2)                 # 6 @ 14x14
    c3 = conv_layer(s2,    filters=16, r=5, s=1)    # 16 @ 10x10
    s4 = max_pool(c3, r_p=2, s_p=2)                 # 16 @ 5x5
    f5 = dense(flatten(s4), 120)
    f6 = dense(f5, 84)
    return output_layer(f6, 10)                     # "Gaussian connections"
```

```
# Choosing padding so that |z| == |x|, given s = 1:
#     N = (|x| + 2p - r)/1 + 1 = |x|   =>   p = (r - 1)/2
# e.g. r = 3 => p = 1;  r = 5 => p = 2.      [external — the notes define
# zero padding by its purpose but never give this formula]
```

## The biological derivation

This is the part L03 was missing. From [[L06-hierarchical-vision]]:

| Visual cortex | Convolutional network |
|---|---|
| [[receptive-field]] (centre-surround) | kernel of size `r` |
| **simple cell** — edge detector | convolution with a learned filter |
| **complex cell** — translation invariant | **[[pooling]]** |
| **hypercomplex cell** — angles, lengths | deeper convolution over pooled maps |
| V1 → V2 → V3 → V5 | stacked conv/pool blocks |

And the lineage is documented rather than merely asserted: [[david-hubel]] and
[[torsten-wiesel]] measured simple and complex cells; [[kunihiko-fukushima]]
implemented them as S-cells and C-cells in the [[neocognitron]]; [[yann-lecun]]
made that trainable end-to-end with [[backpropagation]].

See [[ann-brain-correspondence]] — this is one of only two correspondences in
the module that survive scrutiny.

## …and where it breaks

> **CNNs require [[weight-sharing]], which real neurons cannot do.**

The very feature that makes the CNN work is the one part of it with no
biological counterpart. L06 takes this seriously and proposes two repairs:
[[data-augmentation]] and [[dynamic-weight-sharing]].

## Where it appears in the module

- [[L03-computational-neural-networks]] — p6, one line under "Structures".
- [[L06-hierarchical-vision]] — p6–p9, the substance.

## Open questions / gaps

- **No training procedure is given for the CNN itself.** The notes give the
  forward pass in detail and say nothing about how the filters are learned —
  presumably [[backpropagation]], but the gradient of a shared weight (the sum
  over all positions) is exactly the biologically problematic operation, and
  connecting those two facts would have strengthened the lecture's own argument.
- **"Gaussian connections"** in LeNet's output layer are labelled and never
  explained.
- **No [[activation-function]] appears** anywhere in the convolutional
  formalism — `z_i` is a bare weighted sum.
- Padding is defined by its purpose (`|x| = |z|`) but the formula for `p` is
  never given.

## See also

[[neocognitron]] · [[locally-connected-network]] · [[pooling]] ·
[[weight-sharing]] · [[dynamic-weight-sharing]] · [[data-augmentation]] ·
[[receptive-field]] · [[simple-complex-hypercomplex-cells]] ·
[[neural-similarity-and-dot-product]] · [[multi-layer-perceptron]] ·
[[recurrent-neural-network]] · [[ann-brain-correspondence]] ·
[[intelligent-behaviour]] · [[lime]]


## L10 ? 3D kernels, and the module's only negative result

[[multichannel-cnn|MCCNN]] extends convolution into time:

```
1D kernel  [A B C]                 a line
2D kernel  3?3                     a plane
3D kernel  a cube                  a stack of frames ? space AND time
```

*"Convolution and pooling along image stacks in the first layer"*, so a single
filter can respond to a spatiotemporal pattern.

The verdict, from [[deep-network-tradeoffs]]:

> **3D kernel: no significant advantage over 2D kernels.**

> [!note] The only negative result in the module
> Every other architecture in ten lectures is presented as working. This one is
> reported as not worth its cost, and the reason is structural: a 3D kernel has a
> **fixed temporal extent**, so it is time-*aware* over a window but not
> time-*invariant*. A gesture performed more slowly falls outside the window. A
> recurrent state has no such limit ? hence [[cnn-lstm]].

### Fixed kernels inside a learned network

MCCNN's edge channels use **Sobel filters** ? hand-specified 3?3 convolutions ?
alongside learned ones. [[L06-hierarchical-vision]] argued that oriented edge
detectors are exactly what a trained first layer **discovers**, recapitulating
[[simple-complex-hypercomplex-cells|simple cells]]. MCCNN puts the answer in by
hand.

That is L05's [[hybrid-architecture|learned ? specified]] axis appearing *within
a single layer*: defensible as a way of saving data, and squarely against the
module's own argument for learned features.

### Convolution still is not distinguished from correlation, or normalised

Now used across L06, L08 and L10 without either point being made. L10's
[[motion-intensity-profile|SSIM]] is the first properly normalised comparison
measure in the module.

## L12 ? the hierarchy reused as an explanation

L12 returns to the layer stack as the basis for
[[explainable-ai|explainable deep learning]]:

> **Use of trained feature extractors to model data representation in CNN.**
> 1st layer = edges ? 2nd = parts ? 3rd = objects ? feature vector.
> **Each layer increases the level of abstraction of the modelled data.**

Identical to L06's ladder, now offered as evidence that the network is legible
rather than that it is brain-like ? see [[levels-of-abstraction]] for why those
are different claims.

L12 also supplies the CNN-specific XAI method, [[class-activation-map]], which
works only because convolutional feature maps preserve spatial layout. This is
the practical pay-off of [[weight-sharing]] that L06 did not mention: shared
weights make a feature map a *map*, and a map can be shown to a human.
