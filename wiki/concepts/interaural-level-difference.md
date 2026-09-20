---
type: concept
title: Interaural Level Difference (ILD)
sources: [L05]
tags: [audition, localisation]
---

# Interaural Level Difference (ILD)

> **Level difference (dB) of the signal at the two ears.**

The second azimuth cue. The head blocks sound on its way to the far ear, so the
near ear hears it louder.

## Why it only works at high frequencies

Because the blocking depends on wavelength — see [[acoustic-shadow]]:

- **Short wavelength** (high frequency) ⇒ the head is a large obstacle ⇒
  **high difference for high frequencies**
- **Long wavelength** (low frequency) ⇒ the wave diffracts around the head ⇒
  **small difference for low frequencies**

⇒ **Judgement dominated by [[interaural-time-difference]] in low frequencies,
ILD in high frequencies.** Duplex theory.

## How it is computed

**ILD in the Lateral Superior Olive (LSO)** — see [[auditory-pathway]].

The LSO receives **excitation from the ipsilateral ear and inhibition from the
contralateral ear** (the sign flip is performed by the MNTB, drawn in the p6
diagram and never explained). Its firing rate is therefore a running
subtraction: loud on my side minus loud on the other side. `[external for the
excitation/inhibition detail; the notes give only the location.]`

Where the MSO needs the exquisite timing machinery of the [[jeffress-model]],
the LSO needs only a comparison of levels — so this cue *can* be carried by a
rate code. It is the one part of the localisation problem that a conventional
[[mcculloch-pitts-neuron]] could handle, and the excitatory/inhibitory weight
signs of [[excitatory-and-inhibitory-neurons]] are exactly the mechanism.

## Complementarity

The two cues are not redundant — they cover different halves of the spectrum,
and between them the whole audible range:

| | ITD | ILD |
|---|---|---|
| Quantity | arrival time | intensity |
| Unit | ms (really µs) | dB |
| Best at | low frequencies | high frequencies |
| Nucleus | MSO | LSO |
| Needs spike timing? | **yes** | no |

Only [[hybrid-spiking-localisation-network]] uses both;
[[hybrid-acoustic-tracking]] uses ITD alone.

## Unclear in the source

- No numbers: no dB range, no crossover frequency between the two regimes.
  `[external]` The transition is usually placed around 1.5 kHz.
- The notes never explain *why* ILD fails at low frequencies beyond the
  wavelength sketch — diffraction is drawn but not named.

## See also

[[interaural-time-difference]] · [[acoustic-shadow]] · [[auditory-pathway]] ·
[[excitatory-and-inhibitory-neurons]] ·
[[hybrid-spiking-localisation-network]] · [[azimuth-and-elevation]] ·
[[L05-robot-sound-localisation]]
