---
title: Cortico-collicular Architecture
type: system
tags: [multimodal, architecture, neuroscience]
sources: [L07]
created: 2026-09-21
updated: 2026-09-21
status: developing
---

# Cortico-collicular architecture

> **From SC to cortico-collicular arch.**
> ? [[L07-crossmodal-processing]], p6

The lecture's final architecture, extending the [[superior-colliculus]] model
with a cortical level and a feedback path.

- **Top-down: cortical**
- **Bottom-up: collicular subcortical alignment**
- **Multimodal units show the highest activation for congruent audiovisual
  patterns**
- That congruence drives a **top-down modulatory projection to bias the
  integration at the subcortical layer**

## The structure

```
            ????????????????? cortical multimodal layer ????????????????
            ?   highest activation for CONGRUENT audiovisual patterns  ?
            ????????????????????????????????????????????????????????????
      top-down  ? modulatory projection        bottom-up    ?
       (bias)   ?                                           ?
            ????????????????? subcortical (SC) ?????????????????????????
            ?      integration layer ? alignment of the two maps       ?
            ???????????????????????????????????????????????????????????
                ?                                          ?
           visual map                               auditory map
          (superficial SC)                        (inferior colliculus)
```

Two loops at two speeds. The subcortical one is fast and reflexive; the cortical
one is slower, sees more context, and **biases** rather than overrides. See
[[top-down-modulation]].

## In the terms of the lecture's own taxonomy

[[fusion-strategies]] classifies networks as early, intermediate or late fusion.
The cortico-collicular arch is **intermediate fusion, twice, with feedback** ?
unimodal representations are learned, fused subcortically, fused again
cortically, and the second fusion modulates the first.

The notes give the taxonomy on p2?3 and the architecture on p6 and never place
one in the other. Doing so is the clearest way to see what is novel here: the NN
taxonomy has no category for **fusion that feeds back into an earlier fusion**.

## Pseudocode

```
function cortico_collicular(x_v, x_a, n_steps):
    gain = 1.0
    for step in 1 .. n_steps:
        # bottom-up: subcortical integration, biased by the current estimate
        sc = sc_integrate(x_v, x_a, gain)

        # cortical: unimodal representations, then multimodal units
        cortical = multimodal_layer(net_v(x_v), net_a(x_a))

        # congruence is read off activation MAGNITUDE, not content
        congruence = activation_level(cortical)

        # top-down: set the bias for the next pass
        gain = f(congruence)
    return sc
```

```
# What the loop buys:
#   pass 1 ? SC fuses naively; cortex judges whether the result is coherent
#   pass 2 ? if congruent, fusion is reinforced; if not, the modalities are
#            allowed to segregate (see unity-assumption)
#
# i.e. the unity assumption is not tested once and for all. It is settled
# iteratively, by a loop between a fast fuser and a slow critic.
```

## Assessment

This is the most architecturally ambitious proposal in the module so far, and
also the least supported. Every earlier system in L05?L07 arrives with either
equations or a validation; this one has neither. It is drawn, described in five
bullet points, and summarised.

What makes it worth a page anyway is the **shape** of the idea: a reflexive
subcortical integrator and a knowledgeable cortical one, coupled by a modulatory
rather than a driving projection. That is a genuine design pattern ? compare
[[hybrid-architecture]] from L05, which argued that systems should be
heterogeneous, with different parts built in different ways. Here the
heterogeneity is in **speed and authority**, which is a new axis.

## Unclear in the source

- **No equations, no training procedure, no results.**
- **How the top-down signal enters** the subcortical computation ? gain,
  additive bias, threshold shift? Not stated.
- **Whether the loop runs within a trial or across learning.**
- **"Collicular subcortical alignment"** ? *alignment* presumably refers to map
  registration, raised on p5 and never pursued. The connection is not made.
- The **superior temporal sulcus**, named on p3 as the cortical MSI site, is not
  identified with the cortical layer here, though it presumably is it.

## See also

[[superior-colliculus]] ? [[top-down-modulation]] ? [[fusion-strategies]] ?
[[gated-multimodal-unit]] ? [[histogram-based-som]] ? [[unity-assumption]] ?
[[hybrid-architecture]] ? [[multisensory-integration]] ?
[[network-architectures]] ? [[L07-crossmodal-processing]]
