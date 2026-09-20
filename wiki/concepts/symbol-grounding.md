---
type: concept
title: Symbol Grounding
sources: [L04]
tags: [language, embodiment, semantics]
---

# Symbol Grounding

How a word gets attached to a thing.

L04 never uses the phrase *symbol grounding problem*, but **grounding** is the
word it repeats most, and the question it poses on p6 is exactly that problem:

> **How can we ground embodied sensory stimuli in embodied concepts and
> therefore, language?**

## The two levels in L04

| Level | Source phrase | Mechanism |
|---|---|---|
| **Basic grounding** | *"Grounding of words in perception and the imitator's production"* | the imitator observes, mimics, and **learns action and names simultaneously** |
| **Higher-order grounding** | *"Grounding of names and concepts of new actions"* | [[transfer-learning]] — combined actions with a linguistic description in natural language |

Both are delivered by the [[imitation-network]].

The key phrase is **"learns action and names simultaneously"**. Grounding is not
a separate labelling step applied after the action is learned; the co-occurrence
of word and sensorimotor experience *is* the binding. This is
[[hebbian-learning]] logic at the level of whole behaviours.

## The quality criterion

L04's result line is stronger than accuracy:

> Grounding: **result not just accurate but also contextually correct.**

A model that outputs the right word for the right object is accurate. A model
that outputs the right word *for this situation* has something closer to
meaning. The notes do not expand on how this was measured.

## Three routes offered in the lecture

1. **[[cross-modal-stimuli-prediction]]** — ground a modality in another
   modality. A [[self-organising-map]] shared between visual and auditory
   encode/decode paths, so *concepts activate latent sensory representations*.
2. **[[imitation-network]]** — ground words in one's own motor production.
3. **[[multi-layer-associator]]** — ground auditory word form in articulatory
   form by Hebbian co-activation.

Note that (1) and (3) ground **sign in sign** — sound in image, sound in
articulation. Only (2) grounds sign in **world**, via the body. By the strict
form of the problem, only (2) actually counts. The lecture does not draw this
distinction.

## Why it matters for the rest of the module

Grounding is the reason the module can claim [[gpt]] is not the end of the
story. GPT manipulates text with no referents at all; on the embodiment thesis
([[embodied-language-representation]]) it has no meanings, only distributions.
L04 presents GPT anyway, immediately after arguing for grounding, and does not
comment. That silence is the lecture's most interesting gap — see
[[ann-brain-correspondence]].

## See also

[[embodied-language-representation]] · [[imitation-network]] ·
[[cross-modal-stimuli-prediction]] · [[transfer-learning]] ·
[[compositionality-of-language]] · [[hebbian-learning]] ·
[[L04-embodied-language-processing]]
