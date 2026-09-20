---
title: Motion history image (MHI)
type: concept
sources: [L10]
tags: [gesture, vision, representation, coding]
updated: 2026-09-21
---

# Motion history image (MHI)

A way of collapsing a sequence of frames into **one image** whose intensity
encodes **how recently** motion occurred at each pixel.

[[multichannel-cnn|MCCNN]] is applied to it:

> **Can be used for temporal data ? learning the shape of the Motion History
> Image (MHI) ? extended application to dynamic gestures.**

```
# The standard construction (the notes give the name and the use, not the rule)
def mhi(frames, tau):
    H = zeros_like(frames[0])
    for t, f in enumerate(frames):
        moving = (abs(f - frames[t-1]) > threshold)
        H[moving]  = tau                  # fresh motion: maximum value
        H[~moving] = maximum(H[~moving] - 1, 0)   # older motion decays
    return H
```

The result is a single greyscale image in which a moving limb leaves a **fading
trail** ? bright where it is now, dim where it was.

## Why it is a clever trick and also a confession

**Clever:** it turns a temporal problem into a spatial one. A CNN cannot see
time, but it can see the *shape of a trail*, and the shape of the trail is the
trajectory. The whole apparatus of [[convolutional-network|convolution]] and
[[pooling]] then applies unchanged.

**A confession:** you only need this because the network has no memory. The
history has to be pre-computed into the input because the architecture cannot
accumulate it. The lecture says so, in effect:

> **Simple MCCNN architecture does not yet explicitly model the temporal
> domain ? compresses temporal information by 3D convolution.**

And so the next architecture, [[cnn-lstm]], puts the memory **inside** the model
and the input goes back to being a plain sequence.

> [!note] The same move, twice in the module
> L05's [[jeffress-model]] converted a **time** difference into a **place** code
> using delay lines, so that a static comparison could read it out. MHI converts
> a **time** sequence into an **intensity** code so that a static convolution can
> read it out.
>
> Both are *trade time for space so the reader can be memoryless*, and it is
> the module's most reliably recurring engineering idea ? see also
> [[place-cells|topographic codes]].

## Limitations

- **Self-occlusion**: a trail that crosses itself overwrites its own history,
  so cyclic gestures lose information.
- **Direction is ambiguous** in the decay pattern alone unless the decay is read
  carefully.
- **Fixed `tau`** sets a maximum gesture length.

None of these is raised in the source.

## See also

- [[multichannel-cnn]] ? [[motion-intensity-profile]] ? [[cnn-lstm]] ?
  [[static-and-dynamic-gestures]]
