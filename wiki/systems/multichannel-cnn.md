---
title: Multichannel convolutional neural network (MCCNN)
type: system
sources: [L10]
tags: [gesture, vision, convolution, architecture]
updated: 2026-09-21
---

# Multichannel CNN (MCCNN)

> **Extension to CNN with 3D kernel.**
> **Convolution and pooling along image stacks in 1st layer.**
> **Application of Sobel filters ? edge detection along the horizontal and
> vertical image direction.**

## The three channels

| Channel | Content |
|---|---|
| **Motion image** | the [[motion-history-image|MHI]] |
| **Sobel X** | horizontal edges |
| **Sobel Y** | vertical edges |

Each runs through its own convolution stack, and the three are joined at a
**classifier**:

```
                Motion image     Sobel X     Sobel Y
L1:             cubic convolution    ?           ?
                max pooling          ?           ?
L2:             convolution          ?           ?
                find feature         ?           ?
                     ???????????? Classifier ?????
```

Pipeline: **Frames F??F_N ? motion representation ? MCCNN ? classifier ?
gesture.**

## Kernel dimensionality

```
1D kernel:  [A B C]                    slides along a line
2D kernel:  [A B C / D E F / G H I]    slides over a plane
3D kernel:  a cube                     slides over a stack of frames
```

The 3D kernel is the whole idea: convolve across **time** as well as space, so a
single filter can respond to a spatiotemporal pattern.

## Pseudocode

```
def mccnn(frames):
    M  = motion_history_image(frames)
    Sx = sobel_x(frames[-1])
    Sy = sobel_y(frames[-1])

    a = maxpool(conv3d(M))      # "cubic convolution" over the image stack
    a = find_feature(conv(a))
    b = conv_stack(Sx)
    c = conv_stack(Sy)
    return classify(concat(a, b, c))     # late fusion of three channels
```

> [!note] Three channels joined at the classifier is **late fusion**
> [[fusion-strategies]] (L07) named this. The channels never interact until the
> final layer, so the network cannot learn a feature that depends jointly on
> motion and edge orientation ? which is exactly the kind of feature a gesture
> needs. Neither lecture connects them.

## The hand-written part

Sobel filters are **fixed 3?3 convolutions**, chosen by a person, inserted into
a network whose entire premise is that convolution kernels should be **learned**.
[[L06-hierarchical-vision]] made the case for learning exactly these:
[[simple-complex-hypercomplex-cells|simple cells]] are oriented edge detectors,
and a trained first layer discovers them.

So MCCNN's first layer is [[hybrid-architecture|part learned, part specified]] ?
L05's axis, applied inside a single layer. Reasonable as engineering (it saves
data), but it is the module's own critique of hand-designed features, in the
architecture arguing against them.

## The verdict the lecture reaches

> **Simple MCCNN architecture does not yet explicitly model the temporal
> domain ? compresses temporal information by 3D convolution. Needs large
> training sets. Computationally demanding: do not always meet real-time
> conditions.**
>
> And, in the DNN balance sheet: **3D kernel ? no significant advantage over 2D
> kernels.**

A negative result, stated plainly. See [[deep-network-tradeoffs]]. It is the
reason [[cnn-lstm]] exists.

## See also

- [[convolutional-network]] ? [[pooling]] ? [[motion-history-image]] ?
  [[cnn-lstm]]
