---
type: concept
title: Geometric Sound Localisation
sources: [L05]
tags: [audition, localisation, geometry]
---

# Geometric Sound Localisation

**Geometric computation (with ITD)** — turning a measured delay into an angle.

## The two formulas

```
a = c · t_ITD          c = speed of sound
Θ = arc cos (a / b)
```

- `t_ITD` — the [[interaural-time-difference]], from
  [[cross-correlation-localisation]]
- `a` — the extra *distance* the sound travelled to reach the far microphone
- `b` — the microphone spacing (the baseline)
- `Θ` — the angle of incidence

The triangle in the notes has the right angle between `a` and the incoming ray,
with `b` as the baseline.

## Worked example (L05, p2)

Given delay `d = 8` samples at sampling rate `r = 32000 1/s`, `c = 340 m/s`,
`b = 15 cm`:

```
a = d · c/r = 8 · 340/32000 = 0.085 m

Θ = arccos(a/b) = arccos(0.085/0.15) = arccos(0.5667) = 55.48°
```

Both figures check out. Note the substitution `t_ITD = d/r` — the correlator
returns a delay in **samples**, so the sampling rate converts it to seconds.

## Pseudocode

```
def angle_from_delay(d, c, r, b):
    # d : ITD in samples (from cross-correlation)
    # c : speed of sound, m/s
    # r : sampling rate, 1/s
    # b : microphone spacing, m

    a <- d * c / r                  # path-length difference in metres

    if abs(a) > b:
        return UNDEFINED            # delay exceeds what the geometry allows

    return arccos(a / b)
```

## What limits it

**Resolution is set by the sampling rate.** One sample of delay is the smallest
distinguishable step, so with `r = 32000 Hz` the quantum is
`340/32000 ≈ 1.06 cm` of path difference — about 4° near broadside, and much
worse near the extremes where `arccos` flattens out. Raising `r` is the only way
to improve it.

**The domain is bounded.** `a` cannot exceed `b`, so `d` cannot exceed
`b·r/c = 0.15 · 32000/340 ≈ 14` samples. A correlator that returns a larger
delay has found a spurious peak — an echo, or a second source. The notes do not
mention this check; the pseudocode above adds it.

## The far-field assumption

The formula treats the incoming wavefront as **planar** — i.e. the source is far
enough away that both microphones see the same arrival angle. Close sources
produce curved wavefronts and the triangle stops being right-angled. `[external]`
This is a reasonable assumption for a robot listening across a room and a bad
one for a source a few centimetres away. Not stated in the notes.

> [!warning] The angle convention is not stated
> `arccos(a/b)` measures `Θ` **from the microphone axis** — 0° is *towards one
> microphone*, 90° is straight ahead. A convention measuring from straight ahead
> would use `arcsin`. The worked answer of 55.48° is consistent with the arccos
> reading, but which physical direction that corresponds to is never said.
> Worth checking against the original slides.

## Where it sits

This is the final step of stage 1 in [[hybrid-acoustic-tracking]]: the
correlator gives `d`, this gives `Θ`, and the [[simple-recurrent-network]] then
predicts where `Θ` will be next.

The biological pathway has no equivalent step — the [[jeffress-model]] never
computes an arccos. It produces a **place code** in which the tuned delay of
each detector *is* the answer, calibrated by experience rather than trigonometry.
That difference is worth holding onto: the algorithm converts, the brain tabulates.

## See also

[[interaural-time-difference]] · [[cross-correlation-localisation]] ·
[[hybrid-acoustic-tracking]] · [[azimuth-and-elevation]] ·
[[jeffress-model]] · [[L05-robot-sound-localisation]]
