---
type: system
title: HuBERT
sources: [L04]
tags: [speech, self-supervised, clustering]
status: stub-by-source
---

# HuBERT

**Hidden-unit BERT** — L04's example of **self-supervised speech
representation**. (The expansion is `[external]`; the notes give only the name.)

## What the notes say

- **Based on BERT.**
- **Learning by listening:**
  - learn the structure of spoken input with **offline clustering**
  - **alternate between clustering and prediction steps** to progressively
    improve the representation
- **Self-supervised**: no reliance on supervised labels, text, etc.
- The pre-trained model can be **adapted to new domains**.

## Pseudocode

The iterative refinement loop is the one idea the notes do give, so it is worth
writing out:

```
# ---- HuBERT-style iterative self-supervised training ----
# audio  : unlabelled speech corpus
# K      : number of discrete units (cluster centroids)

features <- cheap_acoustic_features(audio)     # e.g. MFCCs, for round 0

repeat R times:

    # --- offline clustering: invent the labels ---
    labels <- kmeans(features, K)              # pseudo-label every frame

    # --- prediction: train a masked model against those labels ---
    for each utterance u in audio:
        h      <- encoder(mask_random_spans(u))
        loss   <- sum over masked frames t of  -log p(labels[u][t] | h[t])
        update encoder by gradient descent on loss

    # --- the improved encoder produces better features for the next round ---
    features <- encoder(audio)

return encoder
```

The trick is the bootstrap: the labels are garbage in round 0, but a model
trained to predict garbage consistently still learns phonetic structure, so
round 1's clusters are better, and so on.

> [!note] The masking step is `[external]`
> The notes say "alternate between clustering and prediction" but never say
> *what* is predicted or that spans are masked. Masked-span prediction is
> BERT's defining mechanism and is assumed here. The clustering algorithm
> (k-means) is likewise not named in the notes.

## Why it is in a bio-inspired lecture

Because of **learning by listening**. HuBERT acquires speech structure from raw
audio with no transcription, which is the machine analogue of what an infant
does — the same argument [[imitation-network]] makes for actions. It is the
lecture's nearest thing to a model of **language acquisition**, which the
closing summary names as the goal.

Its relation to [[symbol-grounding]] is worth noting and the lecture does not
note it: HuBERT grounds sound in *sound*, not sound in *world*. It discovers
phonetic categories, not meanings. By the lecture's own embodiment argument
that makes it structure-learning, not grounding.

## See also

[[self-supervised-learning]] · [[gpt]] · [[word2vec]] ·
[[self-organising-map]] · [[transfer-learning]] ·
[[L04-embodied-language-processing]]
