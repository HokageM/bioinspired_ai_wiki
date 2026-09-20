---
title: Static and dynamic gestures
type: concept
sources: [L10]
tags: [gesture, vision, recurrent]
updated: 2026-09-21
---

# Static and dynamic gestures

The division that organises every architecture in
[[L10-gesture-recognition]].

| | **Static** | **Dynamic** |
|---|---|---|
| Definition | **static postures, no temporal information to be recognised** | **dynamic hand and arm movement and trajectories** |
| Example | **"OK"** | **"move to the left"** |
| What matters | **hand shape and finger configuration** | **spatiotemporal pattern recognition** |
| Challenge | **postures with complex backgrounds** | **start and end of isolated or continuous gestures** |
| Models | template matching, elastic graph matching, **SVM, MLP, 2D/3D CNN** | HMMs, **RNN, SOM, growing-when-required networks** |

## The two challenges are genuinely different

**Complex backgrounds** is a *segmentation* problem: the signal is present in
one frame, buried in clutter. It is solved by better features ? which is what a
[[convolutional-network|CNN]] provides.

**Start and end of continuous gestures** is a *segmentation in time* problem,
and no amount of per-frame feature quality solves it. You cannot classify a
gesture before knowing where it begins, and you cannot know where it begins
without some notion of what gestures look like ? a chicken-and-egg that the
lecture states and never resolves.

The two partial answers offered later are [[gesture-phases]] (gestures have a
stereotyped rest ? stroke ? rest shape, so boundaries are detectable) and
[[motion-intensity-profile]] (a scalar curve whose minima are candidate
boundaries). Neither is presented as an answer to this challenge, but both are
one.

## Why the model lists differ

Static models are **classifiers over one vector**. Dynamic models must carry
state:

```
static :  label = f(frame)
dynamic:  h_t   = g(h_{t-1}, frame_t);   label = f(h_T)
```

Everything in the dynamic column ? HMMs, RNNs, and
[[gamma-gwr|Gamma-GWR]] ? is a different answer to *what is `h`, and how does it
update*. HMMs make it a discrete distribution, RNNs a learned vector,
Gamma-GWR a **context vector attached to a competitive unit**.

> [!note] The hybrid is the interesting case
> [[snapshot-model]] runs **both** channels ? a dynamic
> [[cnn-lstm|CNN-LSTM]] on differential images and a static branch on extracted
> key frames ? because real gestures carry information in both, and *"gestures
> with similar motion and hand pose"* defeat either alone.

## See also

- [[gesture-representation]] ? [[gesture-phases]] ? [[multichannel-cnn]] ?
  [[cnn-lstm]] ? [[snapshot-model]]
