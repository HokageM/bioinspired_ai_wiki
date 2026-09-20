---
type: system
title: Hybrid Spiking Localisation Network
sources: [L05]
tags: [audition, spiking, robotics, hybrid, localisation]
---

# Hybrid Spiking Localisation Network

L05's second system, and the one that actually uses L02's machinery: a spiking
front end modelled on the [[auditory-pathway]], followed by a conventional
feed-forward classifier.

The lecture's summary lists it as **ITD & ILD in a hybrid spiking NN** — note
that unlike [[hybrid-acoustic-tracking]], this one uses **both** cues.

## The pipeline (L05, p6)

```
                                    Dim. reduction
  L ─┬─→ ┌──────┐                         │
     ╳   │ MSO  │ ──────→ ┌────┐ ─────→ ┌─────────────────┐ ──→ ┌──────────────┐
  R ─┴─→ │ LSO  │         │ IC │        │ Classification  │     │ Motor control│
         └──────┘         └────┘        └─────────────────┘     └──────────────┘

         └──── Spiking NN ────┘         └── Feed-forward NN ──┘
```

Note the crossing arrows on the left: **each of MSO and LSO receives both ears**.
That is forced by the task — you cannot compute a *difference* from one input.

⇒ **Output of MSO & LSO integrated in IC.** The IC stage is annotated
**dimensionality reduction**.

## The stages

| Stage | Computes | Implementation |
|---|---|---|
| MSO | [[interaural-time-difference]] | spiking, coincidence detection ([[jeffress-model]]) |
| LSO | [[interaural-level-difference]] | spiking, excitation vs inhibition |
| IC | integration of both cues | spiking; **dimensionality reduction** |
| Classification | angle, or source identity | feed-forward NN |
| Motor control | head turn | not described |

## Sound encoding

**From sounds to spike trains** — *spikes encoding time and level information.*

The front end is:

```
ear pinna → middle ear → inner ear → auditory nerve
```

producing a [[tonotopic-representation]]. Each frequency channel becomes its own
spike train, and both cues are computed **per channel** — which is what makes
duplex weighting possible ([[acoustic-shadow]]): the low-frequency channels
carry usable ITD, the high-frequency channels carry usable ILD.

## Pseudocode

```
# ---- bio-inspired localisation pipeline ----

def localise(audio_L, audio_R):

    # --- cochlear front end: tonotopic decomposition into spike trains ---
    chan_L <- [ spike_encode(bandpass(audio_L, f)) for f in frequency_channels ]
    chan_R <- [ spike_encode(bandpass(audio_R, f)) for f in frequency_channels ]

    mso_out <- []
    lso_out <- []

    for f in frequency_channels:

        # --- MSO: ITD by coincidence detection across a delay-line array ---
        for i in 0 .. N_delays-1:
            a <- delay(chan_L[f], tau_L[i])
            b <- delay(chan_R[f], tau_R[i])
            mso_out[f][i] <- integrate_and_fire(a + b)    # fires on coincidence

        # --- LSO: ILD by subtracting a sign-inverted contralateral input ---
        # MNTB flips the contralateral excitation into inhibition
        lso_out[f] <- integrate_and_fire( chan_R[f] - via_MNTB(chan_L[f]) )

    # --- IC: integrate both cues, reduce dimensionality ---
    ic <- reduce( concat(mso_out, lso_out) )

    # --- read-out: an ordinary feed-forward net ---
    angle <- feedforward_net(ic)
    motor_control(angle)
    return angle
```

The MSO branch is a spike-domain cross-correlator; the LSO branch is a spike-domain
subtraction. Both are **coincidence/comparison operations over two inputs** —
the same primitive that L02's [[integrate-and-fire]] unit provides, used twice
with different wiring.

> [!warning] The MNTB inversion is reconstructed
> The p6 circuit diagram draws **MNTB** between the ears and the LSO, but the
> notes never say what it does. Its role — converting the contralateral
> excitatory input into inhibition so the LSO computes a *difference* — is
> `[external]`. Without it the LSO would sum rather than subtract, and no ILD
> would be computed. The same applies to **AVCN**, drawn and never defined.

## Why this is the module's best bio-inspired system

Measured against L01's two requirements in [[intelligent-behaviour]] — that the
*representation* and the *processing* both follow the biological principle:

- **Representation:** spike trains carrying time and level information, organised
  tonotopically. Not a convenient abstraction — the actual code the ear uses.
- **Processing:** the same operations, in the same anatomical arrangement, as the
  brainstem.

Nothing in L03 or L04 comes close to this. The rate-coded networks of L03 keep
the *shape* of neural computation while discarding its substance; here the
substance is the point, because the signal is intrinsically temporal.

## The hybrid seam

The system is **not** fully spiking. Everything up to the IC is spiking;
classification is an ordinary feed-forward net. The lecture does not remark on
this, but the seam is where the interesting problem sits: spiking networks are
hard to train with [[backpropagation]], so the pragmatic move is to hand-wire the
biologically-understood part and learn only the read-out.

This is the same argument as [[hybrid-architecture]], applied one level deeper.

## Unclear in the source

- **The classification stage has no stated output.** Angle? Source identity?
  Both?
- **What "dimensionality reduction" at the IC means** is not specified — pooling,
  a learned projection, or simply fewer cells.
- **No training procedure** for either the spiking part or the classifier.
- **AVCN and MNTB are drawn but never defined.**
- **How the per-channel ITD and ILD estimates are combined** into one azimuth is
  not described — this is the duplex-weighting question and it is skipped.

## See also

[[auditory-pathway]] · [[jeffress-model]] · [[spiking-neural-network]] ·
[[interaural-time-difference]] · [[interaural-level-difference]] ·
[[tonotopic-representation]] · [[hybrid-architecture]] ·
[[hybrid-acoustic-tracking]] · [[L05-robot-sound-localisation]]
