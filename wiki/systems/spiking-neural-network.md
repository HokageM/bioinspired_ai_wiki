---
title: Spiking Neural Network
type: system
tags: [spiking, neural-networks, neuroscience]
sources: [L02, L05]
created: 2026-09-20
updated: 2026-09-20
status: solid
---

# Spiking Neural Network

A network whose units communicate by discrete spikes in time rather than by
continuous activity levels. The subject of [[L02-spiking-neural-networks]] and
the module's first fully bio-principled model.

## Biological origin

The lecture's opening justification — what a nervous system buys an organism
(L02, p1):

1. **Selective transmission of signals across distant areas**, enabling more
   complex bodies and brains.
2. **Complex adaptation to environment changes.**

And the licence for modelling at all: the components and behaviour of neurons
are **very similar across animal species**; what develops in a brain is **how
neurons are interconnected** (L02, p1).

From the lecture's summary: *spiking NNs resemble biological information
transmission* (L02, p8).

## Computational form

The defining choice is representational — state is a binary
neurons × time raster, not a vector of levels (see [[neural-coding]]):

```text
# state
S[i][t] in {0, 1}          # did neuron i spike at time t
u[i]    in R               # membrane potential of neuron i

# simulation loop — the general shape shared by all spiking models here
initialise u[i] = u_rest for all i

for t in 0 .. T:
    for each neuron i:

        # 1. integrate incoming spikes, weighted by synapse
        I = sum over j of w[i][j] * S[j][t - delay[i][j]]
        #   or, with [[synaptic-kernel]]s:
        #   I = sum over j of convolve(u_kernel[i][j], S[j])(t)

        # 2. membrane dynamics (see [[continuous-dynamic-neuron]])
        u[i] += dt * ( -(u[i] - u_rest) / tau + R * I )

        # 3. threshold — all-or-none
        if u[i] >= theta[i]:
            S[i][t] = 1
            u[i] = u_reset                  # [[refractory-period]]
        else:
            S[i][t] = 0

# 4. learning, if any, is [[stdp]] over the recorded spike times
```

Two concrete instantiations are given in the module:

| Model | Refractory mechanism |
|---|---|
| [[integrate-and-fire]] | Strong negative feedback $-r_i$ after a spike |
| [[spike-response-model]] | Inhibitory feedback via an exponential function |

Both treat spikes as **Dirac impulses $\delta(\tau)$**; they differ *only* in the
inhibitory feedback (L02, p6).

## What it buys

Access to [[temporal-coding]] — in particular the two schemes a rate-coded
network cannot express at all:

- coincidence / synchronicity across a population,
- delay coding, where stimulus strength becomes first-spike latency.

And a learning rule that reads those timings: [[stdp]].

## Where it appears in the module

- [[L02-spiking-neural-networks]] — the whole lecture.

## See also

- [[mcculloch-pitts-neuron]] — the rate-coded counterpart.
- [[action-potential]] — what a spike is.
- [[hodgkin-huxley-model]] — the detailed biophysical ancestor.
- [[discrete-dynamic-neuron]], [[continuous-dynamic-neuron]] — the intermediate
  steps from static unit to spiking unit.

## L05 — the first application

Three lectures after this page was written, L05 finally *uses* a spiking
network. The task is sound localisation, and it is the first problem in the
module where spikes are not an optional modelling choice.

The reason is [[interaural-time-difference]]: the cue is a difference of
microseconds between the ears. No firing rate resolves that. The signal is
intrinsically temporal, so the code must be too.

| Component | What it does | Page |
|---|---|---|
| Cochlear front end | sound → spike trains, tonotopically ordered | [[tonotopic-representation]] |
| MSO | coincidence detection over delay lines ⇒ ITD | [[jeffress-model]] |
| LSO | excitation minus inhibition ⇒ ILD | [[interaural-level-difference]] |
| IC | integrates both, reduces dimensionality | [[auditory-pathway]] |

The full system is [[hybrid-spiking-localisation-network]].

Note what the coincidence detector requires: a **short membrane time constant**,
so that two spikes arriving close together sum past threshold while two spikes
far apart do not. That is precisely the leak in [[integrate-and-fire]] and the
recovery window of [[refractory-period]] — L02's dynamics turned into a
computation.

> [!note] Still hybrid, not fully spiking
> L05's network is spiking up to the IC and then hands over to a conventional
> feed-forward classifier. The gap below — no training procedure for a spiking
> network — is not closed, it is **sidestepped**: the spiking part is hand-wired
> from anatomy and only the read-out is learned. See [[hybrid-architecture]].

## Open questions / gaps

- **No training procedure for a spiking *network* is given** — [[stdp]] is
  stated as a synapse-level rule, with nothing about network-level objectives,
  readout, or evaluation.
- No mention of the non-differentiability problem that makes spiking networks
  hard to train with gradients.
- No worked example, no dataset, no performance comparison against a rate-coded
  network.
- Hardware (neuromorphic computing) is not mentioned.
