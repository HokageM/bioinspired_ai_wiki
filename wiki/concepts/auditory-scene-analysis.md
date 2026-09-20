---
title: Auditory scene analysis (ASA)
type: concept
sources: [L09]
tags: [attention, audition, perception]
updated: 2026-09-21
---

# Auditory scene analysis (ASA)

> **ASA allows the auditory system to perceive and organise sound information
> from the environment.**
>
> **Auditory attention: localise sound sources and filter out irrelevant sound
> information.**

## The cocktail party effect

> **At a noisy party, a person can concentrate on the target conversation (a
> top-down process, *endo*) and easily respond to someone calling his/her name
> (a bottom-up process, *exo*).**

> [!success] This closes a thread open since L05
> ~~L05 posed the cocktail party problem and left it unsolved: the
> [[jeffress-model]] and [[cross-correlation-localisation]] locate **a** source
> and say nothing about separating **several**.~~
>
> ASA is the answer's name. Note what it adds to L05: localisation is **not
> enough**. Knowing where each talker is does not give you their speech ? you
> still have to decide which acoustic energy belongs to which source, which is
> the grouping problem, and then which source you care about, which is attention.

## The two halves

The definition of auditory attention has two clauses doing different work:

- **localise sound sources** ? L05's problem, solved there
- **filter out irrelevant sound information** ? the new one, and the hard one

And the party example shows the filter cannot be a hard filter: if unattended
streams were discarded, your own name could not interrupt you. Something is
processed in the "ignored" channels. See
[[exogenous-and-endogenous-attention]].

## Relation to vision

| | Visual attention | Auditory attention |
|---|---|---|
| Organised by | space (a [[saliency-map|location map]]) | **streams** ? grouped over time |
| Competition | over locations | over **auditory objects** |
| Selection | [[winner-take-all]] on saliency | object competition |
| Hard problem | conjunction binding | **grouping across time** |

The asymmetry is real: a visual scene is laid out in space and can be sampled at
a location, while an auditory scene is a **single mixed waveform** at each ear
and must be unmixed before anything can be selected. ASA is that unmixing.

## See also

- [[auditory-attention-model]] ? [[auditory-pathway]] ?
  [[cross-correlation-localisation]] ? [[attention]]
