---
title: Gated Multimodal Unit (GMU)
type: system
tags: [multimodal, neural-networks, architecture]
sources: [L07]
created: 2026-09-21
updated: 2026-09-21
status: solid
---

# Gated Multimodal Unit (GMU)

> **Integration at unit level.**
> ? [[L07-crossmodal-processing]], p6

- **Information fusion: creates a new representation out of different
  modalities.**
- **Information flow is controlled by gate neurons `?`, that determine modality
  contribution.**
- **Fully differentiable NN unit.**

## The unit

```
        x_v                      x_t
         ?          ? ?????????????
         ?          ?             ?
         ?          ?             ?
       tanh         ?           tanh
         ?          ?             ?
         ?          ?             ?
       (? ?)                 (? (1??))
          ????????? (+) ??????????
                     ?
                     ?
                     z
```

Written out ? the notes give only the diagram:

```
h_v = tanh(W_v ? x_v)
h_t = tanh(W_t ? x_t)
?   = sigmoid(W_? ? [x_v ; x_t])
z   = ? ? h_v  +  (1 ? ?) ? h_t
```

`?` is computed **from both inputs**, so the unit decides how much to trust each
modality *based on what both of them say*. The `?` and `(1??)` pairing makes the
blend convex: contributions always sum to one, so the unit reweights rather than
rescales.

## This is the GRU gate

The convex-combination gate `z = ??a + (1??)?b` is **exactly** the update gate of
L03's [[gated-recurrent-network]], where it blends the previous hidden state with
a candidate state.

| | Gated recurrent unit (L03) | Gated multimodal unit (L07) |
|---|---|---|
| Blends | `h_{t?1}` with candidate `h?_t` | modality `v` with modality `t` |
| Along | **time** | **modalities** |
| Gate from | current input and previous state | both modalities |
| Learns to control | how much history to keep | how much of each sense to trust |

Same arithmetic, different axis. Neither lecture mentions the other, and the
module has now used this gate twice without naming it as a reusable pattern:
*a learned convex blend is how a network chooses between sources.*

## Why gating is the right operation

[[fusion-strategies]] lists four simple types ? concatenation, multiplication,
sum, function. The GMU is the **function** case, and it is built on
multiplication for a specific reason: multiplication is the only one of the four
that can express *conditional* contribution. Sum and concatenation weight the
modalities the same way for every input. A gate can say "trust vision **here**,
trust audio **there**".

That makes the GMU the learned, per-input counterpart of
[[optimal-cue-integration]]'s reliability weights. Where the statistical model
computes `w_v = (1/?_v?)/(1/?_v? + 1/?_a?)` from known variances, the GMU
*learns* a function that outputs the same kind of quantity from the data. The
lecture puts these on pages 2 and 6 and does not connect them.

And it matches the p6 summary's proposal that *levels of neural activity in the
unimodal layer may provide the reliability of each modality* ? `?` is precisely
a learned read-out of that.

## Pseudocode

```
function gmu_forward(x_v, x_t, W_v, W_t, W_s):
    h_v = tanh(matmul(W_v, x_v))
    h_t = tanh(matmul(W_t, x_t))
    s   = sigmoid(matmul(W_s, concat(x_v, x_t)))    # the gate
    return s * h_v + (1 - s) * h_t
```

```
# Fully differentiable, so it trains end-to-end by backpropagation:
#   dz/ds  = h_v - h_t        the gate learns from how much the two differ
#   dz/dh_v = s               a closed gate passes no gradient to that branch
#
# Note the consequence: a modality the gate has learned to ignore stops
# receiving gradient, so it stops improving. Gating can self-reinforce a
# premature preference.  [external] ? not discussed in the notes.
```

## Unclear in the source

- **No equations** ? the unit is given as a diagram only. Everything written
  above is reconstructed from it, and the `W` matrices are not in the notes.
- **Is `?` a scalar or a vector?** Per-feature gating (`?` a vector, `?`
  elementwise) is far more expressive than a single scalar weight per modality.
  The diagram does not say, and it matters.
- **What feeds `?`** is drawn as arrows from both inputs, but whether from the
  raw inputs or from `h_v, h_t` is ambiguous.
- **Only two modalities.** The `?` / `(1??)` construction does not extend to
  three; that needs a softmax. Not mentioned, though the lecture elsewhere
  discusses visual, auditory **and** somatosensory input.
- **No task, no results, no comparison** against the simple fusion types.

## See also

[[gated-recurrent-network]] ? [[fusion-strategies]] ?
[[optimal-cue-integration]] ? [[multisensory-integration]] ?
[[top-down-modulation]] ? [[cortico-collicular-architecture]] ?
[[activation-function]] ? [[network-architectures]] ?
[[L07-crossmodal-processing]]
