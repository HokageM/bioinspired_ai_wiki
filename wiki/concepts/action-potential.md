---
title: Action Potential (Spike)
type: concept
tags: [neuroscience, spiking]
sources: [L02]
created: 2026-09-20
updated: 2026-09-20
status: solid
---

# Action Potential (Spike)

A stereotyped, all-or-none excursion of a neuron's membrane voltage. It is the
unit of activity that every spiking model in the module is built to reproduce.

## Biological origin

The lecture draws the membrane-voltage trace directly (L02, p1):

| Phase | Membrane voltage $u$ (mV) |
|---|---|
| Resting state | $-70$ |
| Threshold | $-55$ |
| Action potential (peak) | $+40$ |

The sequence is: **stimulus → depolarisation → action potential →
repolarisation → [[refractory-period]] → resting state**. The diagram also marks
a *failed attempt* — a sub-threshold bump that does not produce a spike.

Two properties from the notes:

- **Cycle duration ≈ 3–50 ms** (L02).
- **All-or-none:** "once over threshold, then full spike!" (L02). Spike
  *amplitude* therefore carries no information — only spike *timing* and *count*
  can, which is the entire basis of [[temporal-coding]].

Signal path: **stimuli arriving at dendrites are transferred via the axon to the
synapses**, and activity is measured as spikes (L02).

## Computational form

The all-or-none property is why models can treat a spike as a
**Dirac impulse $\delta(\tau)$** — a timestamp with no shape — which is exactly
what [[integrate-and-fire]] and [[spike-response-model]] do (L02).

```text
# all-or-none thresholding, the minimal model of a spike
on each timestep t:
    u <- integrate_inputs(u, t)          # depolarisation

    if u >= THRESHOLD:                   # -55 mV in the lecture's figure
        emit_spike(at=t)                 # full amplitude, always: +40 mV
        u <- RESET                       # repolarisation
        block_input(for=REFRACTORY)      # see [[refractory-period]]
    else:
        pass                             # "failed attempt": no spike at all
```

## Where it appears in the module

- [[L02-spiking-neural-networks]] — introduced on page 1 as the biological
  ground truth for everything that follows.

## See also

- [[refractory-period]] — the phase that models reproduce with negative feedback.
- [[neural-coding]] — what a train of these spikes can mean.
- [[integrate-and-fire]], [[spike-response-model]] — the models that generate them.
- [[rate-coding]] — the abstraction that throws individual spikes away.

## Open questions / gaps

- The ionic mechanism (sodium/potassium channels) is not covered — the notes
  stay at the level of the voltage trace.
- The "failed attempt" is drawn but never discussed.
- The 3–50 ms figure is given without saying whether it covers the spike alone
  or the spike plus refractory recovery.
