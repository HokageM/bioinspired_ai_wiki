---
type: concept
tags: [vision, neuroscience, anatomy]
sources: [L06, L07]
status: solid
---

# Visual pathway

The route visual information takes from the eye into the brain, as given in
[[L06-hierarchical-vision]].

## The main line

```
foveal image
   → left eye / right eye
      → optic nerve
         → optic tract
            → Lateral Geniculate Nucleus (LGN)
               → striate cortex (V1)
                  → extrastriate areas (V2, V3, V4, V5)
                     → ventral / dorsal streams
```

The **superior colliculus** branches off the optic tract in parallel with the
LGN.

## Four subcortical targets

Stimuli from [[the-retina]] project to **four** subcortical regions — only one
of which is about seeing objects:

| Region | Function |
|---|---|
| **Lateral Geniculate Nucleus (LGN)** | input to **V1**; edge detection |
| **Hypothalamus** | **circadian cycle** — the body clock |
| **Pretectum** | **pupillary light reflex** |
| **Superior Colliculus** | **eye movements** |

This is the structurally interesting fact: light entering the eye is not only a
picture. It also sets the body clock, adjusts the aperture, and steers the
camera. Three of the four targets are *control*, not *perception*.

## Visual areas of the occipital lobe

Processing happens at the back of the brain, in the **occipital lobe**, and is
split by feature:

| Area | Responds to |
|---|---|
| **V1** (striate cortex) | edge detection |
| **V2** | more complex patterns, parts of figures |
| **V3** | colour, motion |
| **(V4)** | orientation |
| **V5** | motion, eye movements |

> [!warning] Uncertain in the source
> V4 is written **in parentheses** in the notes, suggesting the lecturer was
> unsure. Motion is assigned to both V3 and V5 without distinction. `[external]`
> V4 is more commonly associated with colour and form, and V5/MT with motion.
> The notes' V3 = "colour, motion" is unusual.

## Why the order matters

The area list is the biological justification for depth in
[[convolutional-network]]: edges, then parts, then wholes. The
[[neocognitron]] was explicitly built to mirror it, and every deep vision model
since inherits the shape.

The same layered-by-increasing-abstraction pattern appears in the
[[auditory-pathway]] of [[L05-robot-sound-localisation]] — cochlea → brainstem
coincidence detection → cortex. Two sensory systems, one architectural idea.

## Related

[[the-retina]] · [[receptive-field]] · [[simple-complex-hypercomplex-cells]] ·
[[two-visual-streams]] · [[auditory-pathway]] · [[levels-of-abstraction]] ·
[[L06-hierarchical-vision]]


## The superior colliculus, developed in L07

This page lists the SC as one of four subcortical targets of the retina, function
**eye movements**. [[L07-crossmodal-processing]] makes it the centre of the
lecture and gives it a far larger role ? see [[superior-colliculus]].

The two accounts are consistent: a topographically organised map of space with
**motor output**, present in all vertebrates, is exactly a machine for deciding
where to point the eyes. L07 adds that the map is *multisensory* ? the same
column is addressed by vision, audition and touch ? so the orienting decision is
made on pooled evidence.

Neither lecture cross-references the other. Note also L07's claim that *"SC is
the main visual brain region"*, which sits awkwardly beside this page's
V1-centred account; see the warning on [[superior-colliculus]].
