---
type: concept
title: Acoustic Shadow (Head Shadow Effect)
sources: [L05]
tags: [audition, localisation, physics]
---

# Acoustic Shadow

> **Interaural level difference (ILD) best for high-frequency sounds**
> ("head shadow effect").

The head is an obstacle. Whether it casts a shadow depends on how the size of
the obstacle compares with the wavelength of the sound.

## The two regimes

```
  short wavelength                      long wavelength
  ·))))))  (O)  |                        ·  )    )  (O)   )
                                   
  ⇒ high difference for               ⇒ small difference for
    high frequencies                    low frequencies
```

- **High frequency** — wavelength shorter than the head. The wave cannot bend
  around it, so the far ear sits in a genuine acoustic shadow. Large
  [[interaural-level-difference]].
- **Low frequency** — wavelength longer than the head. The wave diffracts
  around it and arrives at both ears at nearly full strength. ILD is tiny.

`[external]` The crossover is roughly where the wavelength equals the head
width: at `c = 340 m/s` and a head of ~0.18 m, that is around 1.9 kHz. The
notes give no numbers.

## Duplex theory

⇒ **Judgement dominated by:**

- **[[interaural-time-difference]] in low frequencies**
- **[[interaural-level-difference]] in high frequencies**

The two cues are complementary precisely because the physics that kills one
leaves the other intact. Low-frequency waves wrap around the head (no ILD) but
still arrive at different times (good ITD). High-frequency waves are blocked
(good ILD) but their wavelength is shorter than the head spacing, so a phase
difference no longer identifies a unique delay (bad ITD).

Between them they cover the spectrum. This is why the brain builds **two**
nuclei — MSO and LSO — rather than one; see [[auditory-pathway]].

## Elevation

> *Difference in elevation also leads to different frequency response.*

One line, and the lecture drops the topic. This is the beginning of the
spectral-cue account: the pinna filters sound differently depending on the angle
it arrives from, imprinting elevation-dependent notches on the spectrum.
`[external]` Neither ITD nor ILD can give elevation, because both are symmetric
about the interaural axis — which is why [[azimuth-and-elevation]] promises two
coordinates and every model in L05 delivers one.

## Engineering consequence

A robot with two microphones inherits exactly this physics, and the size of its
"head" sets its usable frequency range. A small robot has a small baseline,
which means both a small maximum ITD and almost no shadow — so localisation gets
harder as the platform shrinks. The notes do not discuss this; the baseline
`b = 15 cm` in [[geometric-sound-localisation]] is roughly human.

## See also

[[interaural-level-difference]] · [[interaural-time-difference]] ·
[[azimuth-and-elevation]] · [[auditory-pathway]] ·
[[L05-robot-sound-localisation]]
