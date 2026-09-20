---
type: concept
title: Cocktail Party Problem
sources: [L05]
tags: [audition, perception, attention]
status: stub-by-source
---

# Cocktail Party Problem

> **Humans and animals have abilities of sound localisation and sound perception
> in auditory cluttered environments.**

L05 states it as the motivating capability and does not solve it.

## What the problem is

In a room with several simultaneous talkers, the two microphones — or two ears —
receive one summed waveform each. Recovering a single speaker from that mixture
requires separating sources that overlap in both time and frequency.

Localisation helps: if you know *where* a voice is coming from, you can weight
that direction and suppress the rest. This is the lecture's implicit argument
for why a robot should localise at all, and it surfaces in the closing summary:

> **Improve speech recognition by user localisation and vision.**

## Where L05 falls short of it

Every system in the lecture assumes **one source**.

- [[cross-correlation-localisation]] returns a **single** delay — the `argmax`
  of the correlation. Two speakers produce two peaks, and the algorithm as
  written reports only the larger.
- [[hybrid-acoustic-tracking]] tracks **one** trajectory.
- [[hybrid-spiking-localisation-network]] outputs **one** angle.

So the lecture opens with the multi-source problem and then solves the
single-source one. The gap is not acknowledged.

The honest reading is that localisation is a *prerequisite* for cocktail-party
listening rather than a solution to it — you need direction before you can use
direction to separate. `[external]`

## Why it belongs in a bio-inspired module

It is a capability where biology remains clearly ahead of engineering, which is
precisely the criterion L01 set out in [[intelligent-behaviour]] — *solve
problems informed by the best problem solvers in nature*. Humans do this
effortlessly with two ears; array-based engineering solutions typically need
many more microphones.

## Lint candidates

- Does any later lecture return to source separation, attention, or
  multi-speaker audio? If not, this page stays a stub and the lecture's opening
  promise goes unredeemed.
- The **front/back ambiguity** noted in [[azimuth-and-elevation]] compounds
  this: even with one source, two directions are confusable.

## See also

[[interaural-time-difference]] · [[auditory-pathway]] ·
[[cross-correlation-localisation]] · [[intelligent-behaviour]] ·
[[azimuth-and-elevation]] · [[L05-robot-sound-localisation]]
