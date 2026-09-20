---
title: Snapshot model (hybrid static/dynamic network)
type: system
sources: [L10]
tags: [gesture, vision, architecture, hybrid]
updated: 2026-09-21
---

# The snapshot model

> **How to deal with similar movement patterns and subtle movement?**

An **extension of the CNN-LSTM architecture**:

> - **Hybrid (static / dynamic) gesture recognition**
> - **Combine temporal modelling with the posture**
> - **Modular & lightweight architecture**
> - **Evaluation on multiple gesture domains: robot commands and co-speech**

## Architecture ? static and dynamic channels

```
                    Gesture sequence
                    ?              ?
        Differential image      Peak detection
                ?                      ?
            CNN-LSTM            Snapshot extraction
                ?                    ?
                    Classifier
```

Input: **isolated gesture sequence**.

| Channel | Sees | Answers |
|---|---|---|
| **Dynamic** ? [[cnn-lstm]] on differential images | how the hand **moved** | *what trajectory?* |
| **Static** ? snapshots at motion peaks | what the hand **looked like** at the stroke | *what posture?* |

The static channel is [[gesture-phases]] and
[[motion-intensity-profile]] put to work: find the peaks, take those frames,
classify the posture.

## Why both are needed

> **Classification boost of indistinctive movements** (*"feinstrukturiert"*)
> **Classification boost of subtle movements ? enhanced HRI**

[[cnn-lstm]] alone was *"good for gestures with motion profile"* and *"decreased
for more subtle gestures"*. A subtle gesture has little trajectory and most of
its information in the **hand shape** ? so the dynamic channel is nearly blind
to it, and a static channel recovers exactly what was lost.

Conversely two gestures can share a hand shape and differ only in motion. The
two channels are **complementary failure modes**, which is the honest reason to
build a hybrid.

```
def snapshot_model(sequence):
    dyn  = cnn_lstm(differential(sequence))            # trajectory
    prof = motion_intensity(sequence)
    snaps = [sequence[p] for p in find_peaks(prof)]    # the strokes
    stat = cnn(snaps)                                  # posture
    return classify(concat(dyn, stat))
```

## Limits

> **Limitations: gestures with similar motion AND similar hand pose.**

Which is the correct limitation to state: the model fails exactly when **both**
channels are uninformative. Nothing in the architecture can help, because there
is no remaining signal in either representation ? you would need context,
[[social-attention|gaze]], or speech.

> [!note] A third fusion instance, and the first with complementary channels
> [[fusion-strategies]] (L07): fuse modalities. [[multichannel-cnn]] (L10): fuse
> three image channels. Here: fuse **two views of the same signal**, chosen
> because each covers the other's failure.
>
> That is closer to [[optimal-cue-integration]] than anything since L07 ? and it
> is once again a **fixed** combination, with no reliability weighting. The
> obvious improvement is a [[gated-multimodal-unit|learned gate]] between the
> channels, which would let the model lean on posture when motion is weak.
> Precisely [[inverse-effectiveness]], for gestures.

## See also

- [[cnn-lstm]] ? [[gesture-phases]] ? [[motion-intensity-profile]] ?
  [[fusion-strategies]]
