---
title: Gamma-GWR
type: system
sources: [L10, L13]
tags: [unsupervised, competitive-learning, recurrent, architecture]
updated: 2026-09-21
---

# Gamma-GWR

> **Extended GWR: neural network with recurrent connectivity.**
> **Neurons consist of a weight vector and a context vector.**
> **Context descriptor is a combination of the previous BMU's weight vector and
> context vector.**
> **Distance function takes weight and context into account, which allows
> learning of temporal sequences.**

## The idea

[[gwr-network|GWR]] is memoryless: the BMU for `x(t)` depends only on `x(t)`.
Gamma-GWR gives each node a **second vector** ? a context ? and makes the
competition depend on both.

```
# each node j has:  w_j  (weight)   and   c_j  (context)

def gamma_gwr_step(x, prev_bmu):
    # the context the network currently expects, from where it has just been
    C = combine(w[prev_bmu], c[prev_bmu])          # rule not given in the source

    d_j = alpha * ||x - w_j||  +  beta * ||C - c_j||      # weight AND context
    b   = argmin_j d_j

    # growth / adaptation exactly as in GWR, now applied to w and c together
    ...
    return b
```

Because the distance includes the context, **the same input arriving from
different histories selects different winners** ? which is the whole requirement
for representing a sequence.

## Why it matters

| Architecture | Memory mechanism |
|---|---|
| [[gated-recurrent-network|LSTM]] | gated cell state, trained by [[backpropagation]] |
| **Gamma-GWR** | context vector, trained by **competition** |

This is the module's **second** way of doing recurrence, and the first that is
**unsupervised**. Everything temporal so far has needed labels and gradients.
Gamma-GWR learns sequences from unlabelled data, growing capacity as it goes.

> [!note] The recurrence trick, ported into competitive learning
> [[gated-recurrent-network]] (L07) and [[gated-recurrent-network|LSTM]] (L05, L10)
> both work by feeding a summary of the past back into the present comparison.
> Gamma-GWR does the same thing with no gradient anywhere: the past enters as an
> extra term in a **distance**, and the distance decides a winner. It is
> [[winner-take-all]] with a memory.

## Application: motion prediction for physical exercises

```
Posture  ?  G^P  ?
                 ??  G^I  ?  Prediction / Feedback
Motion   ?  G^M  ?
```

Two Gamma-GWR networks, one over **posture** and one over **motion**, joined by a
third **integration** network `G^I` that produces the prediction and the feedback
to the user.

> [!note] The same two-channel split as the snapshot model, one lecture-page apart
> [[snapshot-model]] splits a gesture into **posture** and **motion** and fuses
> them at a classifier. `G^P` / `G^M` / `G^I` splits a movement into **posture**
> and **motion** and fuses them at `G^I`. Same decomposition, different learning
> paradigm ? supervised vs unsupervised. The lecture presents them as unrelated.
>
> `G^I` is also the first **learned, unsupervised** join in the module's long
> series of [[fusion-strategies|fusion]] architectures. Previously the only
> learned join was the supervised [[gated-multimodal-unit|GMU]].

Joint positions come from a [[human-pose-estimation]] framework ?
[[openpose]].

## Unclear in the source

- **The combination rule is not given.** *"A combination of the previous BMU's
  weight vector and context vector"* ? linear? weighted how? The name suggests
  a gamma memory (a cascade of leaky integrators) [external], but the notes do
  not say so, and the depth of that cascade would be the key hyperparameter.
- Whether `prev_bmu` means the BMU of the **previous timestep** or a longer
  trace is not stated.
- The weighting between the weight term and the context term in the distance is
  not specified.
- No evaluation.

## See also

- [[gwr-network]] ? [[self-organising-map]] ? [[gated-recurrent-network|LSTM]] ?
  [[human-pose-estimation]]

## L13 — in GDM, and the summary

> **CL on sequences with Gamma-GWR** ? **GDM: hybrid model with memory replay**

Gamma-GWR is the temporal member of the family, and [[growing-dual-memory|GDM]]'s
**"temporal synapses"** — drawn as a neuron with a `t−1` connection, giving
*"spatiotemporal (context) learning with recurrent GWR"* — are the same idea
inside the dual-memory architecture.

> [!warning] The relationship is not stated
> Whether GDM's "temporal synapses" **are** Gamma-GWR's context descriptors, or a
> different recurrent mechanism, is never said. The two appear on separate pages
> of the same lecture. See [[L13-continual-learning]].

With [[gwr-network|GWR's base algorithm]] now written out in full (L13, p5), the
delta that makes it *Gamma*-GWR — how context vectors enter the distance used to
pick the BMU — is the one piece still missing from an otherwise complete
specification.
