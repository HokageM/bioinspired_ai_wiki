---
title: CNN-LSTM for dynamic gestures
type: system
sources: [L10]
tags: [gesture, vision, recurrent, architecture]
updated: 2026-09-21
---

# Hybrid CNN and LSTM for dynamic gestures

The division of labour is stated explicitly:

> **CNN:** learning of significant visual features ? invariance, spatial
> properties
> **LSTM:** variations in performance ? gesture types, across subjects

Datasets: **commands for HRI** and the **ChaLearn** benchmark.

## Architecture

```
Differential image sequence
   ? first convolution ? max pooling ? second convolution ? max pooling   [CNN]
   ? (x?, x?)  ? LSTM ? LSTM ?  A / B / C                                 [LSTM]
```

The input is a **differential** image sequence ? frame-to-frame differences ?
so the CNN sees *change* rather than *content*, and the LSTM integrates those
changes over time.

```
def cnn_lstm(frames):
    D = [frames[t] - frames[t-1] for t in range(1, len(frames))]
    feats = [maxpool(conv(maxpool(conv(d)))) for d in D]    # CNN per frame
    h = None
    for f in feats:
        h = lstm_step(h, f)                                  # LSTM over time
    return classify(h)                                       # many-to-one
```

## Why this is the right decomposition

The CNN supplies **invariance in space** ? the same gesture performed 30 cm to
the left should give the same features, which is what
[[weight-sharing]] buys. The LSTM supplies **invariance in time** ? the same
gesture performed slowly or quickly should give the same class, which is what a
gated recurrent state buys.

Neither architecture can supply the other's invariance, which is why
[[multichannel-cnn|MCCNN]]'s attempt to get both from 3D kernels gave *"no
significant advantage over 2D kernels"*: a 3D kernel has a **fixed temporal
extent**, so it is not time-invariant at all ? it is merely time-*aware* over a
window.

## Configurations

> **Can be tuned in various ways due to its cell state.**
> **Prediction at the last timestep (many-to-one config) vs prediction at each
> timestep (one-to-one).**

| | **Frame-level CNN-LSTM** | **Sequence-level CNN-LSTM** |
|---|---|---|
| Training method | **online training** | **mini-batch training** |
| Data preparation | **supports varying-sized sequences** | **requires slicing and padding to a fixed length** |
| Classification | **requires post-processing to produce sequence classifications based on frame predictions** | **one classification per gesture sequence** |

Each column pays for the other's convenience. Frame-level handles real gestures
(which vary in length) but needs a post-processing rule to turn per-frame votes
into one answer ? an unspecified [[behaviour-coordination|combination step]].
Sequence-level gives one clean answer but must distort the data to a fixed
length first.

> [!note] Padding a gesture to a fixed length destroys its timing
> And timing is the signal. This is the cost of mini-batching, stated nowhere in
> the module, and it is why the frame-level variant exists at all.

## Results

> **(+) Good performance for gestures with motion profile.**
> **Decrease for more subtle gestures and ChaLearn gestures.**

No numbers. The failure case ? subtle movement ? is what motivates
[[snapshot-model]].

## See also

- [[gated-recurrent-network|LSTM]] ? [[convolutional-network]] ?
  [[snapshot-model]] ? [[motion-intensity-profile]]
