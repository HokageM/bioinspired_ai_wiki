---
type: concept
tags: [training, convolution, plausibility]
sources: [L06, L08, L10]
status: developing
---

# Data augmentation

The **first** of the two mechanisms [[L06-hierarchical-vision]] proposes for
bridging the gap between a plausible [[locally-connected-network]] and a
well-performing [[convolutional-network]]. See [[weight-sharing]] for the
problem.

## The mechanism

- **Done by showing a locally connected network multiple translations of the
  same image — simultaneously.**
- **This makes neurons in a channel react similarly to the same input.**

The logic: a convolutional layer gets translation equivariance *by construction*
because it reuses one filter. A locally connected layer cannot. But if every
position is shown the same content — because the image is presented at every
offset at once — then every position's private weights are driven by the same
statistics, and they should converge on similar values.

The constraint `w_1 = w_4` is replaced by a *pressure* toward `w_1 ≈ w_2`,
`w_3 ≈ w_4`.

## The honest assessment

The notes flag two costs in orange:

- ⚠ **Requires longer training times**
- ⚠ **Only small performance improvement**

So it works, slightly, expensively. Which is why the lecture goes on to
[[dynamic-weight-sharing]].

## Pseudocode

```
function train_with_translation_augmentation(net, images, offsets, epochs):
    for epoch in 1..epochs:
        for img in images:
            # the "simultaneously" is the point: one update sees all offsets
            batch = [ translate(img, dx, dy) for (dx,dy) in offsets ]
            loss  = 0
            for view in batch:
                loss += criterion(net.forward(view), label(img))
            net.update(gradient(loss))       # weights at every position get
                                             # driven by the same content
```

Contrast the version that does *not* work as well — presenting offsets in
separate updates:

```
    for view in batch:
        net.update(gradient(criterion(net.forward(view), label(img))))
```

Here each update only moves the positions that happened to be stimulated, and
there is no term coupling them. The notes' emphasis on **simultaneously** is
doing real work.

## Note on scope

Data augmentation is in general a much broader technique — rotation, scaling,
flipping, colour jitter, noise — used chiefly to combat
[[overfitting-and-underfitting]] by enlarging an effective dataset. `[external]`

**L06 uses it for a different and narrower purpose:** not to prevent
overfitting, but to *induce a symmetry in the weights*. The augmentation is
restricted to **translations** specifically, because translation is the symmetry
that weight sharing encodes. That is a genuinely interesting reframing — the
augmentation is a soft substitute for an architectural constraint.

Compare [[regularisation]] and [[dropout]] from L03, which are also mechanisms
that constrain a network's weights without hard-wiring the constraint.

## Unclear in the source

- **How many translations, and over what range?** Not stated.
- **"Neurons in a channel"** — the word *channel* is used here for the first and
  only time, and is not defined for a locally connected network, which does not
  obviously have channels in the convolutional sense.
- **Small performance improvement relative to what** — to the unaugmented
  locally connected net, or closing what fraction of the gap to the CNN? No
  numbers are given.

## Related

[[weight-sharing]] · [[dynamic-weight-sharing]] · [[locally-connected-network]]
· [[convolutional-network]] · [[regularisation]] · [[dropout]] ·
[[overfitting-and-underfitting]] · [[batch-vs-online-training]] ·
[[L06-hierarchical-vision]]


## Reused for grasping (L08)

[[neural-grasp-learning]] applies this directly:

> - **Randomised cutouts of distractor objects**
> - **Semantic information of target object** (e.g. red, round, tomato)

The first is augmentation in L06's sense ? synthesising harder inputs from the
ones available, here to bridge from clean single-object scenes to clutter.

The second is **not augmentation**. Attaching a semantic description to the
target adds a new *input channel*; it does not transform an existing input. The
lecture files both under the same heading.

> [!note] Augmentation as a patch for a self-limited dataset
> The grasp cycle can only generate samples from positions NICO can already
> reach and release. Cutouts widen the *visual* distribution but cannot widen the
> *kinematic* one ? the hard configurations remain unreachable by the data
> collector. Augmentation covers half the gap, and the notes do not say which
> half.


## L10 ? the data problem, restated

[[deep-network-tradeoffs]] lists **data-hungry and time-consuming** as the first
cost of deep networks, which is the condition augmentation exists to relieve.
L10's answer is different, and more radical: rather than manufacture more data,
**shrink the input**.

> **Benefit from new preprocessing technology: skeletal data provides necessary
> body joints.**

A skeleton from [[openpose]] is roughly 25 joint coordinates where an image is
10? pixels, and it is **already invariant** to clothing, lighting and skin tone ?
i.e. it supplies for free the invariances that augmentation tries to teach by
brute force.

| Strategy | How invariance is obtained |
|---|---|
| Augmentation (L08) | show the network many transformed copies |
| Skeletal preprocessing (L10) | **discard** the varying information before the network sees it |

> [!note] The cost is moved, not removed
> OpenPose is itself a large supervised network trained on a large annotated
> dataset. The data was paid for once, elsewhere, by someone else ? which is
> [[transfer-learning]]'s bargain in a different guise. The source presents
> skeletal input as a clean win.
