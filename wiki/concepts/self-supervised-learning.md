---
type: concept
title: Self-Supervised Learning
sources: [L04]
tags: [learning-paradigm, nlp, speech]
---

# Self-Supervised Learning

Learning from unlabelled data by **constructing the targets out of the data
itself**.

## In L04

Two models are described this way:

- **[[gpt]]** — *generative pre-training*: guess the **next** word given
  context; the notes label this **unsupervised** and **auto-regressive**.
- **[[hubert]]** — *self-supervised speech representation*: **no reliance on
  supervised labels, text, etc.**; learns the structure of spoken input with
  offline clustering, alternating clustering and prediction steps.

[[word2vec]] belongs here too, though L04 does not label it so: predicting a
centre word from its context is the same trick.

## Why it is a fourth paradigm

L03's [[learning-paradigms]] gave three: supervised, unsupervised,
reinforcement. Self-supervised does not fit cleanly into any of them:

| | Labels supplied by a teacher? | Loss against a target? |
|---|---|---|
| Supervised | yes | yes |
| Unsupervised | no | no |
| **Self-supervised** | **no** | **yes** |

It is **unsupervised in its data** and **supervised in its machinery**. The
targets are free — they are just other parts of the input — but once
constructed, training is ordinary gradient descent on a prediction error.

> [!note] Terminology drift in the source
> L04 calls GPT's pre-training **"unsupervised"** while calling HuBERT
> **"self-supervised"**, for what is structurally the same arrangement. Both
> usages are common in the literature; the wiki treats them as one category
> here and notes the inconsistency.

## The general recipe

```
# ---- self-supervised learning, generically ----
for each unlabelled example x:
    # 1. destroy part of the input
    x_visible, x_hidden <- split(x)     # mask spans / drop the next token /
                                        # remove the centre word
    # 2. train the model to reconstruct what was destroyed
    prediction <- model(x_visible)
    loss       <- error(prediction, x_hidden)
    update model by gradient descent on loss
```

The design choice is entirely in `split`. Predict the next token → [[gpt]].
Mask random spans and predict cluster IDs → [[hubert]]. Hide the centre word →
CBOW; hide the context → skip-gram ([[word2vec]]).

## The biological angle

It is the closest thing in modern deep learning to **learning by prediction**,
which is a serious account of what cortex does — the brain is constantly
predicting its next input and learning from the mismatch. `[external]`

L04's own phrase for HuBERT, **"learning by listening"**, is the honest version
of the claim: an infant acquires phonology without transcripts, from raw audio
plus prediction. In that narrow sense self-supervision is the most biologically
defensible training scheme in the module — far more so than
[[backpropagation]]'s labelled targets.

What it still lacks is a **local** update rule. The objective is plausible; the
credit assignment is not. See [[ann-brain-correspondence]].

## See also

[[learning-paradigms]] · [[gpt]] · [[hubert]] · [[word2vec]] ·
[[transfer-learning]] · [[autoencoder]] · [[ann-brain-correspondence]] ·
[[L04-embodied-language-processing]]
