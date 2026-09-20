---
title: Refractory Period
type: concept
tags: [neuroscience, spiking]
sources: [L02]
created: 2026-09-20
updated: 2026-09-20
status: solid
---

# Refractory Period

The interval after an [[action-potential]] during which a neuron cannot readily
spike again.

## Biological origin

Marked on the lecture's membrane-voltage trace as the phase after repolarisation,
during which the voltage undershoots before returning to the resting state
(L02, p1). Functionally it is what stops a neuron from emitting a continuous
stream of spikes while a strong stimulus persists.

## Computational form

The module treats the refractory period as **the** design problem of spiking
models, and both spiking models solve it with negative feedback — which is also
the only thing that distinguishes them (L02, p6):

| Model | Mechanism |
|---|---|
| [[integrate-and-fire]] | Strong negative feedback $-r_i$ applied after a spike, to prevent continuous spike emission |
| [[spike-response-model]] | Inhibitory feedback modelled by an **exponential function**, bringing the neuron's activation to a large negative value after a spike |

The lecture's summary of the pair: *both* treat spikes as functions of time
(Dirac impulses); they *differ* in the inhibitory feedback (L02).

```text
# hard refractoriness (integrate & fire style)
if spiked_at(t):
    a <- a - r                # strong negative feedback, fixed magnitude
    ignore_input(until = t + refractory_length)

# soft refractoriness (spike response style)
# feedback decays, so excitability recovers smoothly
a(t) <- a(t) - sum over past spikes t_f of  R * exp(-(t - t_f) / tau_r)
# large negative immediately after the spike, fading back towards 0
```

The two give different behaviour: the hard version imposes a strict maximum
firing rate; the exponential version makes a second spike *progressively* easier
as time passes, so strong input can still drive fast firing.

## Where it appears in the module

- [[L02-spiking-neural-networks]] — pages 1 (biology) and 6 (both models).

## See also

- [[action-potential]]
- [[integrate-and-fire]], [[spike-response-model]]
- [[temporal-coding]] — refractoriness bounds the maximum firing rate, and so
  bounds what a frequency code can express.

## Open questions / gaps

- The notes do not distinguish absolute from relative refractory period.
- No value is given for the refractory length, nor for $r_i$ or the decay
  constant of the exponential feedback.
