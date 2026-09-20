---
title: OpenPose
type: system
sources: [L10]
tags: [vision, gesture, convolution, architecture]
updated: 2026-09-21
---

# OpenPose

A **bottom-up**, multi-person, real-time [[human-pose-estimation|HPE]]
framework. Used in [[L10-gesture-recognition]] to supply joints to
[[gamma-gwr]].

## Architecture ? two branches, repeated over stages

> **Multi-stage CNN. Branch ? produces L: part affinity fields. Branch ?
> produces S: part confidence maps. F = feature maps. Loss after each stage.**

```
Image ? VGG-like trunk ? F (feature maps)

Stage 1:    S? = ??(F)                 part CONFIDENCE maps  ? where joints are
            L? = ??(F)                 part AFFINITY fields  ? which joints link
              ? loss f_S? , f_L?

Stage t:    S? = ??(F, S^{t-1}, L^{t-1})
            L? = ??(F, S^{t-1}, L^{t-1})
              ? loss f_S? , f_L?

Final:      Part confidence maps ?
                                 ?? BIPARTITE MATCHING ? parsing results
            Part affinity maps   ?
```

## The two outputs

**Part confidence maps (S)** ? one heat map per joint type. A peak says *there is
a left elbow here*. This alone is useless with several people in frame: which
elbow belongs to which shoulder?

**Part affinity fields (L)** ? one **vector field** per limb type. At each pixel
it encodes the direction along which that limb runs. Integrating the field
between a candidate shoulder and a candidate elbow scores whether they are
connected in the same body.

**Bipartite matching** then pairs joints into limbs using those scores, and the
limbs assemble into people.

```
def openpose(image):
    F = trunk(image)
    S, L = None, None
    for stage in range(T):
        S, L = rho(F, S, L), phi(F, S, L)      # refined jointly each stage
    joints = [peaks(S[k]) for k in joint_types]
    limbs  = bipartite_match(joints, score=lambda a, b: integrate(L, a, b))
    return assemble(limbs)
```

> [!note] Affinity fields are the whole idea and the source does not say why
> The problem bottom-up HPE must solve is **grouping**, not detection. Affinity
> fields are a representation of *pairwise association* laid out in the image
> plane ? a topographic code for relations rather than for features.
>
> That is one more entry in the module's [[place-cells|ordered-population code]]
> series (seven now), and the most abstract: the coded variable is not a stimulus
> property at all, but a **binding**.
>
> And grouping-by-association is precisely
> [[auditory-scene-analysis|Gestalt grouping]] from L09, done in vision, by a
> learned field instead of by hand-written principles. The lectures are
> consecutive; neither connects them.

## Loss at every stage

Intermediate supervision ? each stage is trained to produce the correct maps, not
just the last. [external] The stated reason is vanishing gradients in a very deep
stack; the source gives the fact without the reason. Compare
[[vanishing-gradient-problem]], which the module raised for recurrent networks
only.

## Unclear in the source

- **Bipartite matching** is named but not defined ? the graph, the objective and
  the solver are all unstated.
- The number of stages T is not given.
- How affinity fields are scored between two candidate joints is not described.
- No accuracy figures, no dataset, no comparison against top-down methods.

## See also

- [[human-pose-estimation]] ? [[gamma-gwr]] ? [[convolutional-network]]
