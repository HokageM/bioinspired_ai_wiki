---
title: "L02 — Spiking Neural Networks"
type: lecture
tags: [neuroscience, neural-networks, spiking, plasticity, coding]
sources: [L02]
source_file: raw/lectures/BioinspiredAIWdh1und2.pdf
lecture_date: 2024-01-18/19
created: 2026-09-20
updated: 2026-09-20
status: solid
---

# L02 — Spiking Neural Networks

## Summary

The module's first technical lecture, and a long one — seven of the eight scanned
pages. It runs the full ladder from biology to algorithm: what a nervous system
buys an organism, what an [[action-potential]] is, then two modelling traditions
that diverge from the same starting point — **rate coding** (activity *level*)
leading to the [[mcculloch-pitts-neuron]] and connectionism, versus **binary /
temporal coding** (activity *frequency and timing*) leading to
[[spiking-neural-network]]s and computational neuroscience. It then develops the
static rate-coded unit (similarity, separability, encoding, architectures),
makes it dynamic ([[discrete-dynamic-neuron]], [[continuous-dynamic-neuron]]),
then spiking ([[integrate-and-fire]], [[spike-response-model]]), and finishes
with learning: [[hebbian-learning]] and its temporally asymmetric successor
[[stdp]].

The organising contrast of the lecture is **static vs temporal**: a rate-coded
unit's output is determined by its current input, and everything after the
halfway point is an attempt to escape that.

## Key ideas

### Why nervous systems (L02)
1. **Selective transmission of signals across distant areas** — which is what
   permits more complex bodies and brains.
2. **Complex adaptation to environment changes.**

What makes brains differ is *not* their parts: the components and behaviour of
neurons are very similar across animal species. Development in the brain is
**how neurons are interconnected** (L02). This is the licence for the whole
connectionist programme — if the units are generic, structure is where the
intelligence lives.

- **[[action-potential]]** — all-or-none spike; once over threshold, a full
  spike. Cycle ≈ 3–50 ms. Stimuli arriving at dendrites are transferred via the
  axon to the synapses; activity is measured as spikes.
- **[[refractory-period]]** — the recovery window after a spike.
- **[[excitatory-and-inhibitory-neurons]]** — the two types.
- **[[neural-coding]]** — the fork in the road: level vs frequency/timing.
  Branches: [[rate-coding]] and [[temporal-coding]].
- **[[mcculloch-pitts-neuron]]** — $y_i = \phi(A_i) = \phi(\sum_j w_{ij}x_j - \theta_i)$,
  with the [[activation-function]] $\phi$; threshold absorbed into a bias unit.
- **[[neural-similarity-and-dot-product]]** — a neuron's output measures the
  similarity between its input pattern and its weight pattern.
- **[[linear-separability]]** — a neuron cuts the input space into $A \ge 0$ and
  $A < 0$; the threshold shifts the boundary off the origin.
- **[[local-vs-distributed-representation]]** — grandmother cells vs feature
  codes.
- **[[network-architectures]]** — feed-forward, feed-forward multilayer,
  recurrent, fully connected.
- **[[discrete-dynamic-neuron]]** — leaky recurrent activation,
  $a_i(n+1) = \mu_i a_i(n) + \sum_j w_{ij}x_j(n)$.
- **[[continuous-dynamic-neuron]]** — the ODE form,
  $\frac{da_i}{dt} = -\frac{1}{\tau}a_i(t) + \sum_j w_{ij}x_j(t)$; a
  simplification of the [[hodgkin-huxley-model]], read as an RC circuit.
- **[[synaptic-kernel]]** — each synapse contributes with different temporal
  characteristics; input is convolved with per-synapse kernels $u_{ij}$.
- **[[integrate-and-fire]]** and **[[spike-response-model]]** — the two spiking
  models. Both treat spikes as Dirac impulses $\delta(\tau)$; they differ in how
  inhibitory feedback is modelled.
- **[[temporal-coding]]** — three spike encoding schemes: frequency code,
  temporal coincidence/synchronicity, delay coding.
- **[[hebbian-learning]]** — "cells that fire together, wire together";
  $\Delta w_{ij} = x_j y_i$. Suffers from self-amplification and is temporally
  symmetric.
- **[[stdp]]** — Hebbian learning with temporal asymmetry, so weights can now
  *decrease*. Plasticity increases most when the presynaptic neuron fires shortly
  before the postsynaptic neuron.
- **[[donald-hebb]]** — did not assume the existence of synaptic weakening.

### The lecture's own summary (L02, final page)
- Spiking NNs resemble biological information transmission; the activation state
  of a neuron is approximated by firing rate.
- Temporal coding as opposed to static learning (input determines output).
- Learning based on synaptic correlation (Hebb's rule).
- The concept of spike-timing dependent plasticity.
- Different models: **rate-coded**, **integrate & fire**, **spike response**.

## New pages created

Concepts: [[action-potential]], [[refractory-period]],
[[excitatory-and-inhibitory-neurons]], [[neural-coding]], [[rate-coding]],
[[temporal-coding]], [[activation-function]],
[[neural-similarity-and-dot-product]], [[linear-separability]],
[[local-vs-distributed-representation]], [[network-architectures]],
[[synaptic-plasticity]], [[hebbian-learning]], [[stdp]], [[synaptic-kernel]]

Systems: [[mcculloch-pitts-neuron]], [[spiking-neural-network]],
[[discrete-dynamic-neuron]], [[continuous-dynamic-neuron]],
[[integrate-and-fire]], [[spike-response-model]], [[hodgkin-huxley-model]]

Entities: [[donald-hebb]]

## Pages updated

[[index]], [[overview]], [[L01-introduction-to-bio-inspired-ai]]

## Connections

- **Back to [[L01-introduction-to-bio-inspired-ai]]:** L01's second requirement —
  *learn, represent and process based on bio-inspired principles* — is exactly
  what separates the two branches of [[neural-coding]]. A rate-coded network is
  bio-*motivated*; a spiking network is bio-*principled*, because it keeps the
  representation (spike times) biological.
- **Forward:** the module's stated model list (rate-coded / integrate & fire /
  spike response) is the vocabulary later lectures will assume. [[stdp]] is the
  unsupervised learning baseline that later supervised and evolutionary methods
  will be contrasted against.

## Unclear in the source

> [!warning] Probable error in the notes
> The notes write the sigmoid as
> $\phi(A_i) = \frac{1}{1+e^{-kA_i}} = \tanh(kA_i)$.
> The two are **not** equal. The logistic function is bounded in $(0,1)$, $\tanh$
> in $(-1,1)$; the actual relation is $\tanh(z) = 2\sigma(2z) - 1$. The lecture
> most likely presented them as two examples of sigmoid-shaped functions and the
> "=" is a note-taking compression. Recorded on [[activation-function]].

- **Notation drift in [[integrate-and-fire]]:** the margin formula mixes $u_i$
  and $a_i$ — $\tau\frac{d}{dt}u_i = -a_i + RI(t)$, then $u_i(t) = \theta$ for
  fire & reset. The two symbols are clearly meant to be the same membrane
  variable.
- **Grid/place cells are not revisited.** L01's example plays no part in L02.
- **The "failed attempt" label** on the spike diagram (a sub-threshold bump that
  does not trigger a spike) is drawn but never discussed in text.
- **A squid doodle** appears in the bottom-right margin of page 1 next to the
  dendrite/axon sketch. Almost certainly a mnemonic for the squid giant axon
  (the preparation Hodgkin and Huxley used), which would connect to
  [[hodgkin-huxley-model]] on page 5 — but the notes never state this, so it is
  recorded here as inference, not content.
- **$\mu = 0.7$ activation-decay plot** on page 4 is sketched with axis points
  but no values, so the decay curve cannot be reconstructed exactly.
- **[[hodgkin-huxley-model]] is named but never explained** — it appears only as
  "the thing [[continuous-dynamic-neuron]] is a simplification of".
