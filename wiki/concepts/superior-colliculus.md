---
type: concept
tags: [multimodal, neuroscience, anatomy]
sources: [L07, L06]
status: solid
---

# Superior colliculus (SC)

The midbrain structure at the centre of [[L07-crossmodal-processing]], and the
module's best-specified piece of brain anatomy.

> **Multisensory, localises stimuli, present in all vertebrates, motor output.**

That four-part description is doing a lot of work. *Present in all vertebrates*
makes it ancient and basic. *Motor output* makes it not merely perceptual ? it
is a structure that decides where to look.

## What the notes say

- Primary input is **visual**, in addition to **auditory** and **somatosensory**
- Visual input ? **LGN** ? **SC**
- **SC is the main visual brain region**
- **SC sits right over the IC** (inferior colliculus ? sound localisation)
- **SC is topographically organised:** depending on **where** a stimulus is, it
  generates activity in a **specific column** of the SC
- **Different sensory modalities project to different layers of the SC**

## The architecture: columns for *where*, layers for *which sense*

This is the key structural idea, and it is elegant:

```
                  ? columns = position in space ?
       ?????????????????????????????????????????
 layer ?       ?       ?  ???  ?       ?       ?  visual
 layer ?       ?       ?  ???  ?       ?       ?  auditory
 layer ?       ?       ?  ???  ?       ?       ?  somatosensory
       ?????????????????????????????????????????
                         ?
                    motor output
```

Every modality carries its own **map of the same space**, and the maps are
stacked **in register**. A single event in the world lights up the *same column*
in every layer. Integration then requires no computation at all ? it is a
consequence of the wiring.

Two things follow directly:

- **[[spatial-principle]]** is not a rule the SC obeys, it is what the geometry
  *does*. Stimuli close in space share a column and reinforce; displaced stimuli
  occupy different columns and one suppresses the other.
- **Map registration** ? getting the visual and auditory maps aligned ? becomes
  the whole learning problem. The notes raise it (*"it could learn map
  registration"*) and drop it.

## Continuity with L06

[[L06-hierarchical-vision]] listed the superior colliculus as one of the **four
subcortical targets** of the retina, with the function **eye movements**, and
said nothing more. L07 makes it the centrepiece. The two accounts fit together ?
a topographic map of space with motor output *is* a machine for pointing the
eyes ? but neither lecture mentions the other.

See [[visual-pathway]] for the L06 context and [[auditory-pathway]] for the IC,
which supplies the SC's auditory input and was introduced in L05.

## Pseudocode

```
# The SC as a stack of registered maps
function sc_response(visual_field, auditory_field, column):
    v = visual_layer[column]         # activity from superficial layers
    a = auditory_layer[column]       # activity relayed from IC
    # co-located stimuli land in the SAME column ? they add
    # displaced stimuli land in DIFFERENT columns ? lateral inhibition
    return integrate(v, a) - lateral_inhibition(neighbouring_columns)

function orient(sc):
    return motor_command_for(argmax over columns of sc_response(column))
```

```
# Why registration is the hard part:
#   visual map is retinotopic  ? coordinates move when the EYES move
#   auditory map is head-centred ? coordinates move when the HEAD moves
# Keeping them aligned requires continual recalibration. This is
# presumably why a SOM is proposed for the integration layer.  [external]
```

## Two levels of MSI

The lecture distinguishes:

| Level | Structure |
|---|---|
| **Subcortical midbrain** | **superior colliculus** |
| **Cortical** | **superior temporal sulcus (STS)**, between auditory and visual cortex |

and then joins them with [[top-down-modulation]] in the
[[cortico-collicular-architecture]].

## Unclear in the source

- **"SC is the main visual brain region"** conflicts with
  [[L06-hierarchical-vision]], which gives that role to V1 and the occipital
  lobe. Probably means *main visual region of the midbrain*. `[external]` In
  primates the SC is chiefly an orienting structure; V1 dominates visual
  processing. Recorded, not corrected.
- **How many layers and columns**, and what the actual map resolution is, are
  never given.
- **The somatosensory input is mentioned once** and never used.
- **Map registration is raised and dropped.**
- The **STS** is drawn and named and gets no further treatment at all.

## Related

[[multisensory-integration]] ? [[spatial-principle]] ?
[[inverse-effectiveness]] ? [[visual-pathway]] ? [[auditory-pathway]] ?
[[histogram-based-som]] ? [[cortico-collicular-architecture]] ?
[[top-down-modulation]] ? [[place-cells]] ? [[tonotopic-representation]] ?
[[L07-crossmodal-processing]] ? [[L06-hierarchical-vision]]
