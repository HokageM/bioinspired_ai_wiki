---
type: concept
title: Embodied Language Representation
sources: [L04, L10]
tags: [language, embodiment, distributed-representation]
---

# Embodied Language Representation

The thesis L04 is named after, and the lecture's central claim:

> **Language is embodied and distributed over several areas of the brain.**

## The three properties

From p3, *language representation*:

- **Distributed semantic maps**
- **Embodied in multi-modal perception**
- **Involves the whole brain**

⇒ **word-webs.**

## The evidence

**Different areas are active during processing of words for different
objects / subjects / body parts.**

The lecture's example: the word **"shark"** produces *activity in the vision
area — because you have seen one.*

This is the whole argument in one observation. If understanding a word
re-activates the perceptual machinery you used when you encountered the
referent, then word meaning is not stored *somewhere* — it *is* the distributed
sensorimotor trace. There is no amodal symbol sitting in a language module.

## Word-webs

A word is not a node but a **web**: a set of co-active cells spread across
whatever modalities the word touches. *Shark* recruits vision; *kick* recruits
motor cortex for the leg; *salt* recruits gustatory areas. `[external for the
latter two examples; only "shark" is in the notes.]`

This is the cleanest instance in the module so far of
**[[local-vs-distributed-representation]]** coming down firmly on the
distributed side — L02 presented the two options neutrally, L04 takes a
position.

## The consequence for models

If meaning is multi-modal and distributed, then:

- A model trained on **text alone cannot ground anything**, because the
  modalities that carry the meaning are missing. This is the standing objection
  to [[word2vec]] and [[gpt]], which L04 presents without raising it.
- A model needs a **shared representational space across modalities** — which is
  exactly what [[cross-modal-stimuli-prediction]] builds with a
  [[self-organising-map]].
- A model needs a **body**, so that motor words have motor content — which is
  what [[imitation-network]] supplies.

So the three biologically-motivated models of L04 are each a direct engineering
response to one clause of this thesis.

## Tension

Embodiment explains grounding but not **[[compositionality-of-language]]**.
Sensorimotor traces tell you what *open* and *left* mean; they do not tell you
how to understand *open left* if you have never seen it. The lecture asserts
both theses and does not reconcile them.

## See also

[[symbol-grounding]] · [[local-vs-distributed-representation]] ·
[[dual-stream-hypothesis]] · [[language-areas-of-the-brain]] ·
[[cross-modal-stimuli-prediction]] · [[compositionality-of-language]] ·
[[L04-embodied-language-processing]]


## L10 ? grounding by paired perception

[[contrastive-language-image-pretraining|CLIP]] adds a third position to the
module's grounding question.

| | Mechanism | Grounded in |
|---|---|---|
| [[word2vec]] (L04) | co-occurrence in text | **nothing** ? text only |
| Embodied representation (L04) | words tied to sensorimotor experience | acting in the world |
| **CLIP** (L10) | contrastive alignment of a shared embedding space | **paired perception** |

CLIP sits between the two poles. It is not embodied ? no action, no body, no
consequences ? but it is not purely distributional either: the representation of
*cup* is constrained by what cups **look like**, because the training objective
forces the image embedding and the caption embedding together.

> [!note] Which half of embodiment does this satisfy?
> L04's argument has two parts: meaning requires (a) **perceptual** content and
> (b) **sensorimotor** content ? knowing what a cup affords, not just how it
> looks. CLIP supplies (a) at enormous scale and none of (b).
>
> Whether that counts as grounding is exactly the open question L04 leaves, and
> L10 introduces the strongest available test case for it without noticing, on a
> slide about gesture recognition.

[[embodiment-and-situatedness]] (L08) would answer no: a system that has never
acted is not situated, and representations without consequences are not grounded.
