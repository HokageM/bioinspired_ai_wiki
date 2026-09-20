---
title: Synaptic Kernel
type: concept
tags: [spiking, dynamics, neuroscience]
sources: [L02]
created: 2026-09-20
updated: 2026-09-20
status: developing
---

# Synaptic Kernel

A per-synapse temporal response function $u_{ij}$: **each synapse contributes
with different temporal characteristics** (L02, p5).

## Biological origin

In the models up to this point, a synapse is a single number $w_{ij}$ — a gain.
The observation here is that real synapses also have a *shape in time*: the same
input spike produces a postsynaptic effect that rises and decays differently at
different synapses. The lecture calls $u_{ij}$ the **temporal characteristic for
the input**.

## Computational form

The mechanism is a **temporal convolution of the input $x$ with the different
kernels $u_{ij}$** (L02, p5):

$$a_i^{(x)}(t) = \sum_j (u_{ij} * x_j)(t)
= \sum_j \int u_{ij}(s)\,x_j(t-s)\,ds$$

```text
# scalar weight (everything before this page)
a_i(t) = sum over j of  w[i][j] * x_j(t)          # gain only, no memory

# synaptic kernel — each synapse has its own impulse response
a_i(t) = sum over j of  convolve(u[i][j], x_j)(t)

# discrete implementation, per timestep:
for each unit i:
    a[i][t] = 0
    for each input j:
        for s in 0 .. KERNEL_LEN:
            a[i][t] += u[i][j][s] * x[j][t - s]   # weighted sum over the PAST
```

The scalar-weight model is the special case $u_{ij}(s) = w_{ij}\,\delta(s)$ —
an instantaneous kernel with no memory. Giving each synapse a kernel means a
unit's response depends on the *arrival history* at each input separately, so
two inputs with equal weight but different kernels are no longer
interchangeable.

This is what lets a single neuron become sensitive to input *order* and
*relative delay*, which is the machinery the [[spike-response-model]] is built
on: there the kernels $u_{ij}(t)$ shape incoming spikes and a further feedback
kernel handles the [[refractory-period]].

## Where it appears in the module

- [[L02-spiking-neural-networks]] — page 5, under "neuron model with different
  input dynamics", bridging the continuous model to the spiking models.

## See also

- [[spike-response-model]] — the model that uses these kernels directly.
- [[continuous-dynamic-neuron]] — the preceding model, where all inputs share
  one time constant $\tau$.
- [[temporal-coding]] — what per-synapse timing sensitivity makes readable.
- [[integrate-and-fire]] — by contrast, keeps a single membrane time constant.

## Open questions / gaps

- **No functional form is given for $u_{ij}$** — the notes sketch a rising-then-
  decaying bump beside each input but state no equation.
- Nothing is said about how kernels would be *learned*; [[stdp]] adjusts weights,
  not kernel shapes.
- The relationship between $u_{ij}$ and the scalar $w_{ij}$ is not spelled out
  (whether the kernel replaces the weight or multiplies it).
