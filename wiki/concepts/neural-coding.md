---
title: Neural Coding
type: concept
tags: [neuroscience, coding, foundations]
sources: [L02, L06]
created: 2026-09-20
updated: 2026-09-21
status: solid
---

# Neural Coding

The question of **what a neuron's activity means** — and, in this module, the
fork that splits neural modelling into two research traditions.

## Biological origin

Both branches start from the same fact: neurons communicate by
[[action-potential]]s, which are all-or-none. What differs is which property of
a spike train is taken to carry the information.

## The fork (L02, p2)

The lecture draws this as two parallel chains, "modelling neuron communication":

| | **Weight Matrix** | **Binary Matrix Models** |
|---|---|---|
| Coding based on | **activity level** | **activity frequency / timing** |
| ↓ | [[mcculloch-pitts-neuron]] / Perceptron | Spiking neurons |
| ↓ | **Connectionism** | **Computational Neuroscience** |
| Label | *"rate-coded"* | — |
| Visualised as | neuron-ID × neuron-ID matrix of weights (black $<0$, white $>0$) | neurons × time matrix of binary spikes |

This single slide explains the shape of the whole field: the left branch became
machine learning, the right branch became computational neuroscience, and they
differ *at the level of what a number in the model represents*.

Note that the lecture crosses out a word before "Connectionism" — the branch is
labelled only by its endpoint.

## Computational form

```text
# LEFT BRANCH — rate-coded. State is a vector of activity levels.
state:   x[i] in R           # "how active is neuron i", one number, no time
step:    y = phi(W @ x - theta)
storage: W is (N x N) real   # the weight matrix

# RIGHT BRANCH — spiking. State is a matrix over time.
state:   S[i][t] in {0, 1}   # "did neuron i spike at time t"
step:    u[i] <- integrate(u[i], inputs up to t)
         S[i][t] = 1 if u[i] >= theta[i] else 0
storage: S is (N x T) binary # the spike raster
```

The right branch costs a time dimension and buys the ability to represent *when*.
The left branch collapses time and so cannot — this is why
[[discrete-dynamic-neuron]] and [[continuous-dynamic-neuron]] exist, as attempts
to recover temporal structure inside the left branch.

## Where it appears in the module

- [[L02-spiking-neural-networks]] — page 2, the organising diagram of the lecture.

## See also

- [[rate-coding]] — the left branch in detail.
- [[temporal-coding]] — the right branch in detail, with three encoding schemes.
- [[mcculloch-pitts-neuron]], [[spiking-neural-network]] — the two endpoints.
- [[intelligent-behaviour]] — L01's requirement 2 is what makes this fork matter.

## Open questions / gaps

- "Connectionism" and "computational neuroscience" are named as destinations but
  never defined.
- The lecture does not argue for either branch being *correct* about biology; it
  presents them as two modelling choices.


## L06 quietly picks rate coding

[[L06-hierarchical-vision]] states of retinal ganglion cells:

> A light stimulus evokes an action potential in ON-ganglion cells.
> **Frequency increases with sensory strength.**

That is [[rate-coding]], stated as fact, with no mention of the alternative.
The whole of L06's modelling then works in continuous activation values ?
`z_i = ? w_j x_{i+j}` has no time variable at all.

This is worth recording because L02 framed rate versus temporal coding as a
genuine fork between two research traditions, and L05's [[jeffress-model]] made
the case for temporal coding about as forcefully as it can be made: microsecond
spike timing, not firing rate, is what localises a sound.

One lecture later, timing has vanished. Vision gets rate coding without
argument, and the module never explains why the two sensory systems warrant
different treatment. The honest answer is presumably that ITD *requires*
microsecond precision while edge detection does not ? but that is an argument
the notes never make.
