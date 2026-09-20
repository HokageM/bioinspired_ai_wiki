---
title: Spike Response Model
type: system
tags: [spiking, dynamics, neuroscience]
sources: [L02]
created: 2026-09-20
updated: 2026-09-20
status: solid
---

# Spike Response Model

The second of the module's two spiking models (L02, p6). Structurally close to
[[integrate-and-fire]], but built from **kernels** rather than from a single
membrane time constant, and with a *decaying* rather than fixed refractory
feedback.

## Biological origin

Two biological facts are given their own machinery here:

1. Different synapses have different temporal characteristics — handled by the
   [[synaptic-kernel]]s $u_{ij}(t)$ (L02, p5).
2. Excitability recovers gradually after a spike — handled by an exponential
   feedback kernel: **inhibitory feedback modelled by an exponential function,
   bringing neuron activation into a large negative value after a spike
   ([[refractory-period]])** (L02, p6).

## Computational form

Structure (L02, p6):

```
 x1(t) --[ u_i1(t) ]--\
 xj(t) --[ u_ij(t) ]---> [ Σ ] --a_i(t)--> [ ⌐| threshold ] --> [ Π spike ] --> y_i(t)
 xN(t) --[ u_iN(t) ]--/      ↑                       ↑                     |
                             |_____ ρ(τ) kernel _____|_____________________|
                              exponential inhibitory feedback
```

Each input is first shaped by its own kernel, the results are summed to give the
activation $a_i(t)$, thresholded to produce a spike, and the spike feeds back
through a further kernel $\rho(\tau)$ that drives the activation sharply
negative and then decays.

```text
# spike response model
own_spikes = []

for t in 0 .. T step dt:

    # --- 1. input: convolve each input with ITS OWN kernel ---
    a = 0
    for each input j:
        a += convolve(u[i][j], x[j])(t)        # [[synaptic-kernel]]

    # --- 2. refractoriness: sum the feedback kernel over past own spikes ---
    for t_f in own_spikes:
        a += rho(t - t_f)                      # rho < 0, decays exponentially
        # e.g. rho(s) = -R * exp(-s / tau_r)   # [external] exact form not given

    # --- 3. threshold ---
    if a >= theta[i]:
        emit_spike(at = t)                     # Dirac impulse delta(t)
        own_spikes.append(t)
```

The neuron has **no membrane state variable** — $a_i(t)$ is recomputed each step
from the input history and the spike history. That is the structural difference
from [[integrate-and-fire]], which carries $u$ forward.

## Comparison with Integrate and Fire (L02, p6)

The notes state the comparison directly:

| | Integrate and Fire | Spike Response Model |
|---|---|---|
| **Both** | Spikes as functions of time: **Dirac impulse $\delta(\tau)$** | |
| **Different** | Strong negative feedback $-r_i$, fixed | **Inhibitory feedback modelled by an exponential function** |
| Input handling | Single membrane time constant $\tau_i$ | Per-synapse kernels $u_{ij}(t)$ |
| State | Membrane potential $u$ carried forward | Recomputed from spike history |

> *"Both: spikes as function of time: Dirac impulse δ(τ). Different: inhibitory
> feedback."* (L02, p6)

The practical consequence: the exponential feedback makes a second spike
*progressively* easier as time passes, so a strong input can still drive rapid
firing — whereas the fixed $-r_i$ of integrate-and-fire imposes a hard rate
ceiling. This matters for [[temporal-coding]] scheme (a), the frequency code.

## Where it appears in the module

- [[L02-spiking-neural-networks]] — page 6; named in the lecture's closing
  summary as one of the three models to know.

## See also

- [[integrate-and-fire]], [[synaptic-kernel]], [[refractory-period]]
- [[spiking-neural-network]], [[action-potential]]
- [[continuous-dynamic-neuron]] — the shared ancestor of both spiking models.

## Open questions / gaps

- **No equation is given for the model** — only the block diagram. The
  convolution form above is reconstructed from the diagram plus the
  [[synaptic-kernel]] material on page 5.
- The feedback kernel is labelled $\rho(\tau)$ in the diagram (and possibly
  $\eta$ elsewhere in the sketch); its functional form and time constant are not
  stated.
- The notes do not say when to prefer one spiking model over the other.
