---
type: system
title: Jeffress Coincidence Model
sources: [L05]
tags: [audition, spiking, localisation, classic-model]
---

# Jeffress Coincidence Model

**Early Jeffress neuronal coincidence model** — the classical account of how a
brain computes [[interaural-time-difference]], due to [[lloyd-jeffress]].

## The mechanism

Two ingredients:

1. **Delay lines.** Axons from the left ear and the right ear run *towards each
   other*. A spike entering from the left takes progressively longer to reach
   positions further right, and vice versa.
2. **Coincidence detectors.** A row of neurons sits along the shared axis, each
   firing only when it receives input from both sides *at the same moment*.

```
 left ear  ─────────────────────────────────→
            ●    ●    ●    ●    ●    ●            coincidence detectors
 right ear ←─────────────────────────────────

            4    3    2    1    x               (labels from the p2 sketch)
           ← left tract          right tract →
```

If the sound arrives at the left ear first, the left-travelling spike has a head
start, so the two spikes meet *right of centre*. Which detector fires therefore
encodes the ITD.

## Time becomes place

This is the whole idea, and it is worth stating plainly: **a temporal difference
is converted into a spatial code.** Downstream neurons no longer need any
timing machinery — they just read off which cell in the row is active.

The module has now seen this trick three times:

| Lecture | Continuous quantity | Represented by |
|---|---|---|
| L01 | position in the environment | which [[place-cells]] fire |
| L04 | position in a feature space | which unit of a [[self-organising-map]] wins |
| **L05** | **interaural time difference** | **which coincidence detector fires** |

## Pseudocode

```
# ---- Jeffress delay-line coincidence array ----
# N detectors, detector i has delay tau_L[i] on the left input
# and tau_R[i] on the right, with tau_L increasing and tau_R decreasing.

for each detector i in 0 .. N-1:
    tau_L[i] <- i     * dt          # further along the left tract = more delay
    tau_R[i] <- (N-1-i) * dt        # mirror image for the right tract

def localise(spikes_L, spikes_R):
    for i in 0 .. N-1:
        # the detector integrates two delayed spike trains
        a <- delay(spikes_L, tau_L[i])
        b <- delay(spikes_R, tau_R[i])
        response[i] <- coincidence(a, b)      # fires only on near-simultaneous input

    best <- argmax(response)
    itd  <- tau_R[best] - tau_L[best]         # the ITD this detector is tuned to
    return itd
```

The `coincidence` operation is a neuron with a short integration window and a
threshold requiring both inputs — i.e. exactly an [[integrate-and-fire]] unit
with a fast-decaying membrane. Two spikes close together sum past threshold; two
spikes far apart do not, because the first has leaked away. **The refractory and
leak dynamics of L02 are what make coincidence detection work**; see
[[refractory-period]].

## It is a cross-correlator

Compare the loop in [[cross-correlation-localisation]]:

| Algorithm | Jeffress array |
|---|---|
| `for d_i in shifts:` | one detector per `d_i` |
| `s_i = dot(g_shifted, h)` | coincidence count at that detector |
| `argmax over d_i` | whichever detector fires most |

The algorithm iterates over delays; the brain instantiates them in parallel
hardware. **Same computation, different substrate.** This is the module's
cleanest biology↔algorithm correspondence — see [[ann-brain-correspondence]],
where the far weaker *backpropagation ↔ plasticity* claim is recorded.

## Where it lives in the brain

The [[auditory-pathway]] section of L05 places ITD computation in the **Medial
Superior Olive (MSO)**, which is the anatomical home of the Jeffress
arrangement. The lecture presents the model on p2 and the anatomy on p5–p6
without explicitly joining them.

> [!note] `[external]` — the model is contested
> Delay-line coincidence detection is well supported in the barn owl. In
> mammals the evidence points more towards an ITD code based on inhibitory
> timing and population rate differences between the two MSOs, rather than a
> literal delay-line array. The lecture presents Jeffress as *the* mechanism
> without qualification.

## Unclear in the source

- The p2 sketch is labelled with `φ_3`, `ITD`, a low/high **frequency** axis,
  and detectors numbered `4 3 2 1 x` across "left tract" and "right tract".
  The frequency axis suggests one delay-line array **per frequency channel**
  (see [[tonotopic-representation]]), but the notes do not say so.
- No dates, no citation, no first name for Jeffress.
- How the detector output is read out is not described.

## See also

[[lloyd-jeffress]] · [[interaural-time-difference]] ·
[[cross-correlation-localisation]] · [[auditory-pathway]] ·
[[spiking-neural-network]] · [[integrate-and-fire]] · [[temporal-coding]] ·
[[L05-robot-sound-localisation]]
