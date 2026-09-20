---
title: Motor schema
type: concept
sources: [L08]
tags: [robotics, navigation, agents, architecture]
updated: 2026-09-21
---

# Motor schema

> **Behaviour specification for navigation of mobile robots: multiple concurrent
> processes associated with perceptual schemas.**

The cooperative alternative to [[subsumption-architecture]]: instead of letting
one behaviour win, **add all their outputs as vectors**.

## The two schema types

**Perceptual schema** ? *(preprocessing for MS)*

> **Provide environmental information specific for motor schema.**

**Motor schema**

> - **Sensory input, navigation vector as output**
> - **Can have internal state**

Structure, from the notes' diagram: environmental sensors `ES_i` feed perceptual
schemas `PS_i` (which may contain perceptual sub-schemas `PSS`), which feed motor
schemas `MS_i`, whose vectors go to a summation node `?` and then to the motor
field.

## Combination

> **Output of schemas can be visualised as vector / potential field.**
> **Response vector contains direction and strength.**
> **Each schema can be defined independently ? schemas are combined in linear
> combination to single output vector.**

> **`R = ? (G_i ? R_i)`** ? each response `R_i` weighted by gain factor `G_i`

## Pseudocode

```
while True:
    S = read_sensors()
    percepts = [ps(S) for ps in perceptual_schemas]     # task-specific filtering
    R = [ms(percepts) for ms in motor_schemas]          # each -> (magnitude, angle)
    response = sum(G[i] * R[i] for i in range(len(R)))  # linear combination
    actuate(response)
```

Note there is no `if` anywhere. Every schema contributes on every cycle; a
schema "turns off" only by returning magnitude `0`.

## The library of schemas

**Towards-Goal**

```
# Ballistic
V_magnitude = G                 # fixed gain value
V_direction = towards goal

# Guarded
V_magnitude = G          if d >= S
              G * d/S    if d <  S        # d = distance to goal
V_direction = towards goal                # S = radius of influence
```

**Avoid-Obstacle**

```
V_magnitude = 0                    if d >  S     # G = gain
              (S-d)/(S-R) * G      if R < d <= S # d = distance to object
              INF                  if d <= R     # R = radius of object
                                                 # S = sphere of influence
V_direction = away from object
```

**Simple:** Move-Ahead, Noise.
**More complex:** Dodge, Stay-On-Path.

> **Overall robot behaviour is integration of all active schemas** ? *linear
> combination*, e.g. Avoid-Obstacle + Towards-Goal + Stay-On-Path.

> [!success] Both piecewise functions are continuous at their inner boundary
> Guarded Towards-Goal: at `d = S`, `G` and `G?d/S` both give `G`. ?
> Avoid-Obstacle: at `d = S`, `(S?d)/(S?R)?G` gives `0`, matching the outer case. ?
> Checked, because piecewise definitions written by hand usually are not.

> [!warning] `?` at `d ? R` is both discontinuous and unimplementable
> At `d = R` the middle case gives exactly `G`; the notes then jump to `?`. And
> an infinite term inside `R = ?(G_i ? R_i)` annihilates every other schema and
> produces `NaN` on any real machine. What is meant is "large enough to dominate";
> what is written cannot be run.

## Versus subsumption

| | [[subsumption-architecture]] | Motor schemas |
|---|---|---|
| `C` | competitive ? one layer wins | cooperative ? weighted sum |
| Output | an intent some behaviour actually had | possibly an intent **nobody** had |
| Smoothness | switching, discontinuous | smooth |
| Failure | dithering | [[local-minima-problem|minima and cycles]] |

The cost of smoothness is that the blend can be worse than any of its parts.
See [[behaviour-coordination]].

## Where the schemas come from

Nowhere. The gains `G_i` are free parameters that the lecture never tunes,
learns or discusses, and the schema library is hand-written. This is the same
gap as in the [[braitenberg-vehicle]]: the *architecture* is bio-inspired, the
*parameters* are chosen by a person.

## See also

- [[potential-field-navigation]] ? [[local-minima-problem]]
- [[fusion-strategies]] ? the sensory twin of the sum-versus-max choice
