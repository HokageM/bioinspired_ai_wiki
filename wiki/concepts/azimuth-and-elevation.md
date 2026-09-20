---
type: concept
title: Azimuth and Elevation
sources: [L05]
tags: [audition, localisation, geometry]
---

# Azimuth and Elevation

> **Sound has two coordinates: azimuth φ and elevation δ.**

Localising a source means recovering a direction on a sphere centred on the
listener. Two angles suffice:

- **Azimuth φ** — the horizontal angle, swept around the listener.
- **Elevation δ** — the vertical angle, swept over the top.

The p1 sketch marks `φ = 180°, δ = 0°` for a source directly behind, and
`φ = 0°, δ = 0°` straight ahead.

## The asymmetry

The two coordinates are not equally easy, and the lecture — without saying so —
only ever solves one.

| | Azimuth | Elevation |
|---|---|---|
| Cue | [[interaural-time-difference]], [[interaural-level-difference]] | spectral shaping by the pinna |
| Requires two ears? | **yes** | no — it works monaurally |
| Covered in L05 | fully, three different ways | one sentence |

Every system in L05 — [[cross-correlation-localisation]],
[[hybrid-acoustic-tracking]], [[hybrid-spiking-localisation-network]] — returns a
single angle of incidence, i.e. azimuth only.

## Why binaural cues cannot give elevation

Both ITD and ILD are symmetric about the **interaural axis**. Every point on a
cone around that axis produces the *same* time and level difference — the "cone
of confusion". `[external]` Raising a source straight up from directly ahead
changes neither cue: both stay at zero.

So elevation needs a different kind of information altogether, and L05 names it
in passing on p1:

> *Difference in elevation also leads to different frequency response.*

That is the spectral cue — the pinna's folds filter incoming sound
direction-dependently, and the resulting notches identify elevation. See
[[acoustic-shadow]]. The lecture does not develop it.

## Front/back

The same symmetry means azimuth itself is ambiguous front-to-back: a source at
`φ = 45°` and one at `φ = 135°` give identical ITD. Real systems resolve this by
**moving** — turning the head changes the cue for one and not the other.

[[hybrid-acoustic-tracking]] turns the robot's head on every cycle, so it has
the mechanism available, but the lecture never connects head movement to
disambiguation. It is presented purely as *pointing at* the source, not as
*sensing*.

## See also

[[interaural-time-difference]] · [[interaural-level-difference]] ·
[[acoustic-shadow]] · [[geometric-sound-localisation]] ·
[[L05-robot-sound-localisation]]
