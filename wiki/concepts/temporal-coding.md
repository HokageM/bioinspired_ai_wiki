---
title: Temporal Coding
type: concept
tags: [coding, spiking, neuroscience]
sources: [L02, L05]
created: 2026-09-20
updated: 2026-09-20
status: solid
---

# Temporal Coding

Representing information in **when** spikes occur — their frequency, their
coincidence across neurons, or their delay — rather than in an averaged activity
level.

## Biological origin

Because an [[action-potential]] is all-or-none, a single spike can only say
*present* or *absent*: the lecture's framing is that a spike **represents the
presence or absence of a stimulus — a binary decision** (L02, p6). Everything
richer than a binary decision must therefore be carried by timing.

## The three encoding schemes (L02, p6)

The notes list three, each with a margin mnemonic:

### a) Frequency code — *"Pow Pow Pow"*
**Firing rate represents strength of stimulus.** A stronger stimulus produces a
denser burst of spikes from the same neuron.

### b) Temporal coincidence / synchronicity — *"TOGETHER"*
**The number of neurons firing together represents stimulus intensity.** The
information is in how many units align on the same moment, not in any one unit's
rate. This is a *population* code in time.

### c) Delay coding — *"stimulus ↓ delay ↓ fire"*
**Strength of stimulus is encoded in the firing delay of the neuron:** neurons
that receive stronger stimulation fire earlier. Intensity becomes *latency*.

```text
# the three schemes, decoding a stimulus strength s from a spike raster S[i][t]

# a) FREQUENCY CODE — one neuron, count over a window
s_hat = count(S[i][t] for t in [t0, t0 + T]) / T

# b) COINCIDENCE / SYNCHRONICITY — many neurons, one instant
s_hat = count(S[i][t0] for i in population)      # how many fired together

# c) DELAY CODING — one neuron, first-spike latency; inverted
t_first = min(t for t in [t0, t0+T] if S[i][t] == 1)
s_hat   = 1 / (t_first - t0)                     # earlier spike => stronger
```

Scheme (c) is the fastest: it can transmit a value in the time it takes for
*one* spike to arrive, whereas (a) needs a whole averaging window. Scheme (b)
trades neurons for time — it needs a population but resolves in one instant.

> Only (a) is recoverable by [[rate-coding]]. Schemes (b) and (c) are invisible
> to a rate-coded model, which is the concrete answer to "what does a
> [[spiking-neural-network]] buy you".

## Where it appears in the module

- [[L02-spiking-neural-networks]] — page 6, after the two spiking models.
- Named in the lecture's summary as *temporal coding opposed to static learning*.

## See also

- [[neural-coding]] — the fork.
- [[rate-coding]] — the alternative; note that scheme (a) is its bridge.
- [[spiking-neural-network]], [[integrate-and-fire]], [[spike-response-model]]
- [[stdp]] — learning that is itself sensitive to spike timing, and so the
  natural learning rule for a temporal code.
- [[refractory-period]] — sets the ceiling on scheme (a).
- [[interaural-time-difference]] (L05) — the module's first task that *requires*
  a temporal code.

## Vindicated in L05

L02 presented rate vs temporal coding as a fork between two research traditions
([[neural-coding]]) and left the choice open. L05 closes it, for one problem at
least.

Sound localisation turns on the [[interaural-time-difference]] — a gap of tens
of microseconds between the ears. There is no firing rate that resolves ten
microseconds, so the quantity **cannot be rate-coded**. The information is in
the spike times or it is nowhere.

And the brain's solution, the [[jeffress-model]], is a mechanism for reading a
temporal code: delay lines plus coincidence detectors. Note what it then does —
it converts the temporal code into a **place** code, so that downstream neurons
need no timing machinery at all. Which detector fires is the answer.

That is scheme-independent and worth holding onto: temporal codes are used at
the *input*, and converted to something easier as soon as possible. See
[[tonotopic-representation]] for the same pattern applied to frequency.

## Open questions / gaps

- The notes do not say which scheme the brain actually uses, or whether they
  coexist.
- No decoding procedure is given for any of the three — the sketches show
  encoding only.
- Nothing connects the three schemes back to [[hebbian-learning]] or [[stdp]],
  although (c) and STDP are both latency-based.
