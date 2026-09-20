---
type: concept
title: Compositionality of Language
sources: [L04]
tags: [language, generalisation]
---

# Compositionality of Language

*Language is **compositional*** — L04's second claim about language, after
universality.

## What the notes say

- **Humans can infer alternative usages of words and understand unseen usages
  and new compositions of words.**
- Guiding training for compositionality: **evaluation shows that the model can
  infer unobserved compositions similarly to humans, and is even likely to make
  similar mistakes.**

That last clause is the strong result. Matching human *accuracy* shows the model
is good; matching human *errors* suggests it has acquired a similar
generalisation bias — which is a far stronger claim about mechanism.

## Why it is the hard problem

Compositionality is what makes a finite vocabulary infinite. A learner that has
seen *open the red box* and *turn left* must handle *open the left box* without
ever having been shown it. Any model of language acquisition has to explain this
or it is only explaining memorisation.

Two of L04's models are argued to achieve it, by different routes:

| Route | Mechanism |
|---|---|
| [[imitation-network]] | primitives learned separately, then composed — [[transfer-learning]] gives *grounding of names and concepts of new actions* |
| [[word2vec]] | linear structure in the embedding space: `Queen = King + Woman − Man` |

The lecture's own summary line is *"language representations allow composing
complex linguistic constructions from primitives ⇒ compositional language
structure."*

## Tension with the embodied thesis

[[embodied-language-representation]] says meaning is grounded in sensorimotor
experience. But compositionality requires handling combinations that were
**never experienced**. So grounding cannot be the whole story — there must be a
symbolic or algebraic layer that recombines grounded primitives. The lecture
gestures at this with "from primitives" but does not name the mechanism.

This is the classic symbolic-vs-connectionist fault line, and L04 walks along it
without commenting. `[external]`

## Unclear in the source

- p10 opens with a struck-through word before "Humans can infer…". Illegible.
- Which model the "evaluation" refers to is not stated — context suggests the
  [[imitation-network]], but the page does not say.
- "Guiding training for compositionality" implies a specific training
  intervention. What it was is not recorded.

## See also

[[symbol-grounding]] · [[embodied-language-representation]] ·
[[transfer-learning]] · [[word-embedding]] · [[imitation-network]] ·
[[L04-embodied-language-processing]]
