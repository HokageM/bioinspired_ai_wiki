---
type: concept
title: Cross-Modal Stimuli Prediction
sources: [L04, L07]
tags: [multimodal, self-organisation, grounding]
---

# Cross-Modal Stimuli Prediction

L04's architectural answer to *how can we ground embodied sensory stimuli in
embodied concepts and therefore, language?*

## The architecture (L04, p6)

```
 image ──Visual Encoding──→ ┐                ┌ ──Visual Decoding──→ image
                            ├→ ┌───────┐ ←──┤
                            │  │  SOM  │↺   │
                            ├→ └───────┘ ←──┤
 sound ──Auditory Encoding─→ ┘                └ ──Auditory Decoding─→ sound
```

- A single [[self-organising-map]] sits in the middle of two encoder/decoder
  pairs, one visual and one auditory.
- The SOM has a recurrent self-loop (drawn as `↺`).
- Margin notes: **Neighbourhoods form ca…** (cut off — almost certainly
  *categories*) and **Concepts activate latent sensory representation.**

## How it grounds

Both modalities are encoded into the **same** latent space, and the SOM
organises that space topologically. Because it is one shared map:

- **Cross-modal prediction** — encode a sound, read out the map, decode
  *visually*. Hearing "shark" reconstructs the look of a shark.
- **Concepts are map regions.** A neighbourhood on the map is a category, and
  activating it activates the latent sensory representation in *every* modality
  that fed the map.

This is the computational rendering of the "shark" observation from
[[embodied-language-representation]]: *activity in vision area because you have
seen one.* Here that is not an anecdote but a mechanism — the auditory route and
the visual route terminate in the same cells.

## Pseudocode

```
# ---- training ----
# paired data: (image, sound) referring to the same thing
for each (img, snd) in paired_corpus:
    z_v <- visual_encoder(img)
    z_a <- auditory_encoder(snd)

    # both modalities are projected into ONE shared latent space
    z   <- combine(z_v, z_a)

    som.train_step(z)                 # see self-organising-map for the update

    # encoders/decoders are trained to reconstruct through the map
    minimise  norm(visual_decoder(som.project(z))   - img)
            + norm(auditory_decoder(som.project(z)) - snd)

# ---- cross-modal prediction: hear it, see it ----
def sound_to_image(snd):
    z   <- auditory_encoder(snd)
    bmu <- som.best_matching_unit(z)      # land in the shared concept map
    return visual_decoder(som.weights[bmu])
```

> [!warning] This pseudocode is substantially reconstructed
> The notes contain only the diagram and two margin phrases. How the two
> modalities are combined, whether the encoders are trained jointly with the
> map, and what the SOM's self-loop does are all absent from the source. Treat
> the loop above as *a* consistent reading, not *the* lecture's.

## Why a SOM and not an autoencoder

L03 already had the [[autoencoder]], which also produces a latent code. The SOM
adds something the autoencoder lacks: **topology**. Neighbouring latent codes
are neighbouring *units*, so similar concepts are literally adjacent, and
"neighbourhoods form categories" becomes true by construction rather than by
hope. That is what lets a fuzzy or partial input still land in the right
category.

## See also

[[self-organising-map]] · [[symbol-grounding]] ·
[[embodied-language-representation]] · [[autoencoder]] ·
[[local-vs-distributed-representation]] · [[L04-embodied-language-processing]]


## Superseded by L07

[[L04-embodied-language-processing]] used a shared SOM to map between vision and
audition ? encode from one modality, decode into the other.
[[L07-crossmodal-processing]] revisits the same territory with a better-specified
answer, and the difference is worth stating:

| | L04 cross-modal prediction | L07 [[multisensory-integration]] |
|---|---|---|
| Goal | **translate** one modality into another | **fuse** both into one estimate |
| Output | the other modality | a percept, with a reliability |
| Handles conflict | not addressed | [[unity-assumption]], [[spatial-principle]] |
| Handles uncertainty | not addressed | [[optimal-cue-integration]], histograms |

These are different problems. Translation asks *what would this look like?*;
integration asks *what is actually out there?* ? and only the second has to
decide what to do when the senses disagree.

Neither lecture references the other, though both build on the same
[[self-organising-map]]. See [[histogram-based-som]].
