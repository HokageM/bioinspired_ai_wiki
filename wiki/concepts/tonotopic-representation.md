---
type: concept
title: Tonotopic Representation
sources: [L05, L06]
tags: [audition, coding, representation, neuroscience]
---

# Tonotopic Representation

The code produced by the ear: **frequency mapped onto position**.

L05 introduces it in one line, as the output of
`ear pinna → middle ear → inner ear → auditory nerve`. See
[[auditory-pathway]].

## What it means

The cochlea is mechanically tuned along its length — high frequencies excite one
end, low frequencies the other. Each auditory nerve fibre therefore carries the
activity of one narrow frequency band, and neighbouring fibres carry neighbouring
bands. `[external for the mechanical detail; the notes give only the term.]`

The signal reaching the brain is not a waveform but a **bank of band-limited
spike trains**, ordered by frequency.

## Another place code

The module keeps meeting the same representational idea:

| Lecture | Continuous quantity | Encoded by |
|---|---|---|
| L01 | location in the environment | which [[place-cells]] fire |
| L04 | position in a feature space | which unit of a [[self-organising-map]] wins |
| L05 | sound frequency | which auditory nerve fibre fires — *this page* |
| L05 | interaural delay | which coincidence detector fires — [[jeffress-model]] |

Four instances now, across four different systems. The recurring pattern:
**a continuous variable is represented by position in an ordered population**,
with neighbouring values handled by neighbouring cells. Topology is preserved,
which is exactly the property a [[self-organising-map]] is designed to learn —
here it is built in by anatomy rather than acquired.

None of the lectures names this as a general principle. It is arguably the most
consistent representational commitment in the module.

## Why localisation needs it

Because of [[acoustic-shadow]]. The two cues work in different frequency bands:
ITD is reliable low, ILD reliable high. A system that collapsed everything into
a single broadband signal would have no way to apply that weighting.

Tonotopy makes the duplex strategy *possible* — it delivers the frequency bands
pre-separated, so the MSO and LSO can be applied per channel and the results
weighted by band. See [[hybrid-spiking-localisation-network]], where the
pseudocode loops over frequency channels for exactly this reason.

## Relation to coding schemes

It sits alongside, not inside, L02's fork in [[neural-coding]]. Tonotopy says
*which* fibre carries *which* band; [[rate-coding]] and [[temporal-coding]] say
what the spikes *within* a fibre mean. Sound localisation uses both axes at
once: frequency by place, delay by timing.

## Unclear in the source

- The term appears once, undefined and unexplained.
- Whether tonotopy is preserved through MSO, LSO and IC is not stated — it is
  (`[external]`), and it has to be for the per-channel scheme above to work.
- No mention of how many channels a model should use.

## See also

[[auditory-pathway]] · [[neural-coding]] · [[place-cells]] ·
[[self-organising-map]] · [[acoustic-shadow]] ·
[[hybrid-spiking-localisation-network]] ·
[[local-vs-distributed-representation]] · [[L05-robot-sound-localisation]]


## Retinotopy ? the same idea in vision (L06)

[[L06-hierarchical-vision]] presents the visual system's equivalent. Just as the
cochlea maps frequency onto position along the basilar membrane, the retina maps
visual space onto position in V1, and V1 cells additionally tile the space of
edge orientations ([[orientation-tuning]]).

The shared design: **a continuous variable becomes a spatial coordinate**, so
that "which neuron" answers "what value", and neighbouring values are handled by
neighbouring tissue. Once a quantity is laid out this way, operations that would
otherwise need arithmetic ? comparison, interpolation, finding a maximum ?
become local operations between adjacent cells.

This is the fifth instance of the pattern in the module. See [[place-cells]] and
[[overview]].
