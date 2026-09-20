---
type: concept
title: Auditory Pathway
sources: [L05, L06, L07]
tags: [neuroscience, audition, localisation]
---

# Auditory Pathway

L05's answer to *how is sound encoded, and how is the encoded information
processed?*

## Encoding

```
ear pinna → middle ear → inner ear → auditory nerve
```

giving a **[[tonotopic-representation]]**.

**Sound encoding: from sounds to spike trains** — *spikes encoding time and
level information.*

Those two words, **time and level**, are the whole architecture in miniature.
The spike train carries both quantities simultaneously, and the two brainstem
nuclei downstream each read out one of them.

## Processing

| Nucleus | Computes | Page |
|---|---|---|
| **Medial Superior Olive (MSO)** | [[interaural-time-difference]] | [[jeffress-model]] |
| **Lateral Superior Olive (LSO)** | [[interaural-level-difference]] | — |
| **Inferior Colliculus (IC)** | integrates both | — |

> **The IC, for sound localisation, is a major centre of integration in the
> ascending as well as descending auditory pathways.**

⇒ **Output of MSO & LSO integrated in IC.**

> [!note] A self-correction in the source
> On p5 the ITD line was written "Interaural time difference (ITD) in
> ~~Lateral~~ **Medial** Superior Olive (MSO)". The final version is correct:
> ITD → **M**SO, ILD → **L**SO.

## The circuits (L05, p6)

**ITD at MSO:**

```
  ((( ear ──AN──→ AVCN ──→ ┌─────┐ ←── AVCN ←──AN── ear )))
   cochlea                 │ MSO │
                           └──┬──┘
                              ▼
                            ( IC )
```

**ILD at LSO:**

```
  ((( ear ──AN──→ ┌─────┐          ┌─────┐ ←──AN── ear )))
      AVCN        │ LSO │          │ LSO │      AVCN
                  └──┬──┘  MNTB  MNTB └──┬──┘
                     └───────→ ( IC ) ←──┘
```

Both LSOs project to the IC, and each receives a crossed input routed through
the **MNTB** on the opposite side.

> [!warning] AVCN and MNTB are drawn but never defined
> `[external]` **AVCN** = anteroventral cochlear nucleus, the first relay after
> the auditory nerve. **MNTB** = medial nucleus of the trapezoid body, which
> **sign-inverts** the contralateral input — converting excitation into
> inhibition so that the LSO can *subtract* one ear from the other.
> Without the MNTB the LSO would sum its inputs and compute no level difference
> at all. It is the functional heart of the ILD circuit and the notes omit its
> purpose entirely.

## Why two nuclei rather than one

Because of [[acoustic-shadow]]. ITD is reliable at low frequencies and ILD at
high frequencies, and no single mechanism covers both. The brain builds
dedicated hardware for each and merges the results at the IC — which is exactly
what the *integration* in the IC's description is for.

This also explains the **dimensionality reduction** label attached to the IC in
[[hybrid-spiking-localisation-network]]: MSO and LSO between them produce a
large population code — many delays × many frequency channels × two cues — and
the IC collapses it to a direction.

## Relation to L02

This is where L02's machinery lands. [[integrate-and-fire]] units with short
membrane time constants are coincidence detectors; that is the MSO. Signed
inputs from [[excitatory-and-inhibitory-neurons]] make subtraction possible;
that is the LSO. [[temporal-coding]] and [[rate-coding]] were presented in L02
as a *fork between research traditions* — here they appear as **two nuclei in
the same brainstem**, working on the same signal at the same time.

That is a much better argument for temporal coding than L02 itself managed to
make.

## Unclear in the source

- **Descending pathways** are mentioned in the IC description and never
  explained. Top-down auditory feedback is a substantial topic and gets half a
  clause.
- No mention of the **cochlear hair cells** or how transduction actually
  produces spikes — "inner ear → auditory nerve" is the whole story.
- The **auditory cortex** never appears. The pathway stops at the IC.
- How per-frequency-channel estimates are **combined** across the tonotopic
  array is not described.

## See also

[[tonotopic-representation]] · [[interaural-time-difference]] ·
[[interaural-level-difference]] · [[jeffress-model]] ·
[[hybrid-spiking-localisation-network]] · [[spiking-neural-network]] ·
[[acoustic-shadow]] · [[L05-robot-sound-localisation]]


## Parallel with the visual pathway (L06)

[[L06-hierarchical-vision]] describes the [[visual-pathway]] one lecture later,
and the two have the same shape:

| | Auditory (L05) | Visual (L06) |
|---|---|---|
| Transducer | cochlea ? frequency to place | [[the-retina]] ? light to contrast |
| First computation | coincidence detection (MSO) | centre-surround [[receptive-field]] |
| Subcortical relay | brainstem nuclei, IC | LGN, superior colliculus |
| Cortex | auditory cortex | V1 ? V2 ? V3 ? V5 |
| Organising code | [[tonotopic-representation]] | retinotopy, [[orientation-tuning]] |
| Split by question | ITD vs ILD in separate nuclei | [[two-visual-streams]] ? what vs where |

Both systems do serious computation **before** the cortex, both organise their
first cortical stage as an ordered map of a continuous variable, and both split
into parallel pathways specialised by question rather than computing one
general-purpose representation.

Neither lecture mentions the other. The recurrence of the pattern across two
independent sensory systems is stronger evidence for it than either lecture
alone offers.


## The IC feeds the superior colliculus (L07)

[[L07-crossmodal-processing]] adds the next stage. The **inferior colliculus**,
introduced here as an ITD/ILD relay, projects to the
**[[superior-colliculus]]** directly above it:

> **SC sits right over the IC (inferior colliculus ? sound localisation).**

In the L07 model the IC supplies the auditory position estimate `x_a` that is
fused with the visual estimate `x_v` in the SC's deeper integration layer. So the
auditory pathway of L05 does not terminate in auditory cortex as far as
*orienting* is concerned ? a branch of it ends one synapse below the visual map,
in register with it.

This closes a loop opened in L05: the [[hybrid-acoustic-tracking]] robot turned
its head using sound alone, and the obvious question ? what happens when it can
also see? ? is answered here.
