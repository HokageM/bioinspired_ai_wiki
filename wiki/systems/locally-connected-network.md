---
title: Locally Connected Network
type: system
tags: [neural-networks, vision, plausibility, architecture]
sources: [L06]
created: 2026-09-21
updated: 2026-09-21
status: developing
---

# Locally Connected Network

The biologically plausible counterpart to the [[convolutional-network]], and the
starting point for both repairs proposed in [[L06-hierarchical-vision]].

## Definition

A layer in which each output unit reads from a **local** patch of the input —
exactly like a convolutional layer — but in which **each position has its own
private weights**.

```
   locally connected              convolutional
      w_1 ≠ w_4                      w_1 = w_4
```

So it keeps the *locality* of cortical connectivity and drops the
[[weight-sharing]] that cortex cannot implement.

| | Fully connected | **Locally connected** | Convolutional |
|---|---|---|---|
| Each unit sees | all of input | a local patch | a local patch |
| Weights | all distinct | **all distinct** | **shared across positions** |
| Parameters | `n_in × n_out` | `r × n_out` | `r` |
| Translation equivariant | no | **no** | yes |
| Biologically plausible | no | **yes** | **no** |

## The verdict from L06

> **Locally connected networks do not share weights but perform worse than CNNs
> on image classification tasks.**

This one sentence sets up the rest of the lecture. The plausible architecture
loses. The lecture then asks:

> **How to bridge the gap between the biologically plausible locally connected
> network and the well-performing but less plausible CNN?**

**Two mechanisms:**

1. **[[data-augmentation]]** — show the network multiple translations of the
   same image simultaneously, so that every position is driven by the same
   statistics. *Requires longer training times; only small performance
   improvement.*
2. **[[dynamic-weight-sharing]]** — add lateral connectivity and a "sleep
   phase" whose local update rule drives the private weights toward each other.

Both leave the architecture alone and work on making `w_1 ≈ w_4` **emerge**
rather than be imposed.

## Why it loses

Not stated in the notes; the standard account. `[external]`

A locally connected layer must learn "this is an edge" separately at every
position. It has `N` times the parameters and `1/N` of the effective training
data per parameter. An edge seen only in the top-left teaches nothing about the
bottom-right. The convolutional network gets translation equivariance for free
as a structural prior; the locally connected network must learn it from data,
and mostly does not.

This is a prior-versus-data argument, and it is the same shape as the argument
for [[regularisation]] in L03: constraining the hypothesis space helps when the
constraint is true of the world. Translation invariance *is* true of natural
images, so hard-wiring it wins.

## Pseudocode

```
function locally_connected_forward(x, W, r, s):
    # W[i] is a PRIVATE weight vector for output position i
    N = (len(x) - r) / s + 1
    z = []
    for i in 0 .. N-1:
        acc = 0
        for j in 0 .. r-1:
            acc += W[i][j] * x[i*s + j]        # note W[i], not W
        z.append(acc)
    return z
```

```
# The only difference from conv1d, side by side:
#   convolutional     acc += w[j]    * x[i*s + j]
#   locally connected acc += W[i][j] * x[i*s + j]
#
# One index. That index is the whole plausibility argument.
```

```
# Parameter count for an input of length |x|:
#   convolutional     r
#   locally connected r * N   where N = (|x| - r)/s + 1
```

## Unclear in the source

- **No numbers.** "Performs worse" is not quantified — no dataset, no accuracy
  gap, no citation.
- **"Neurons in a channel"** is used when describing [[data-augmentation]], but
  a locally connected network does not obviously have channels in the
  convolutional sense, and the term is never defined here.
- The claim that locally connected networks **are** biologically plausible is
  asserted rather than argued. Cortical connectivity is local, which is the
  evident basis, but the notes do not say so.

## See also

[[convolutional-network]] · [[weight-sharing]] · [[dynamic-weight-sharing]] ·
[[data-augmentation]] · [[network-architectures]] ·
[[multi-layer-perceptron]] · [[regularisation]] · [[ann-brain-correspondence]] ·
[[L06-hierarchical-vision]]
