---
title: Spike-Timing Dependent Plasticity (STDP)
type: concept
tags: [learning, plasticity, spiking, unsupervised]
sources: [L02]
created: 2026-09-20
updated: 2026-09-20
status: solid
---

# Spike-Timing Dependent Plasticity (STDP)

**Hebbian learning with temporal asymmetry** (L02, p8) — the form
[[hebbian-learning]] takes in a [[spiking-neural-network]].

## Biological origin

Where Hebb's rule asks *were both neurons active?*, STDP asks *in what order?*
The quantity that drives the update is (L02, p8):

> the **temporal difference** between the reception of a spike from the
> presynaptic neuron and the emission of a spike from the postsynaptic neuron.

The central empirical claim:

> **Plasticity increases most when the presynaptic neuron fires shortly before
> the postsynaptic neuron.** (L02, p8)

This is causality-shaped. If $j$ fires just before $i$, $j$ plausibly *helped
cause* $i$ to fire, and that synapse is strengthened. If $j$ fires just *after*
$i$, it cannot have contributed, and the synapse is weakened.

## Computational form

Synapse plasticity changes according to an **STDP function of the pre- and
postsynaptic spike timings** (L02). The decisive gain over Hebb's rule:
**weights can now decrease** (L02).

Let $\Delta t = t_{post} - t_{pre}$.

| Ordering | $\Delta t$ | Effect (L02) |
|---|---|---|
| pre before post | $> 0$ | **Increase** (potentiation) |
| post before pre | $< 0$ | **Decrease** (depression) |

```text
# STDP update, event-driven
for each synapse (i, j):
    for each pre-spike time t_pre of neuron j:
        for each post-spike time t_post of neuron i:

            dt = t_post - t_pre

            if dt > 0:                      # pre caused post -> strengthen
                dw = +A_plus  * exp(-dt / tau_plus)
            else:                           # post before pre -> weaken
                dw = -A_minus * exp( dt / tau_minus)
            # [external] the exponential shape and the constants A, tau are the
            # standard STDP window; L02 states only the SIGN and that effect is
            # strongest for small positive dt, not the functional form.

            w[i][j] = w[i][j] + eta * dw
```

Compare directly with [[hebbian-learning]]:

```text
Hebb:  dw = x_j * y_i                  # symmetric in time, sign always >= 0
STDP:  dw = f(t_post - t_pre)          # asymmetric in time, sign can flip
```

That sign flip solves two of Hebb's three problems at once: weakening now exists
(problem 1), and the rule is no longer temporally symmetric (problem 3). It also
partly bounds growth, since a synapse that fires out of order is actively
punished.

## Where it appears in the module

- [[L02-spiking-neural-networks]] — page 8, the final topic; named in the
  lecture's closing summary.

## See also

- [[hebbian-learning]] — the rule this generalises, and the problems it fixes.
- [[synaptic-plasticity]] — the general schema.
- [[temporal-coding]] — STDP is the learning rule appropriate to a code that
  lives in spike times; note the affinity with delay coding (scheme c).
- [[spiking-neural-network]] — the setting that makes STDP meaningful.
- [[action-potential]] — the events being timed.

## Open questions / gaps

- **The STDP function is drawn, not specified.** The notes show up/down arrow
  pairs indicating the sign, but give no equation, no time constants, and no
  window width. The exponential form above is marked `[external]`.
- Nothing says whether the update is applied per spike pair or per pattern.
- No worked example, and no statement of what STDP *learns* — i.e. what
  representation emerges from it.
