---
title: CLIP (Contrastive Language-Image Pretraining)
type: system
sources: [L10]
tags: [multimodal, language, vision, learning]
updated: 2026-09-21
---

# Contrastive Language-Image Pretraining (CLIP)

Three steps, as given:

## 1. Contrastive pre-training

> **Text encoder + image encoder. Matrix of pairs.** Example caption: *"Pepper
> the aussie pup"*.

```
for batch of N (image, caption) pairs:
    I = image_encoder(images)         # N embeddings
    T = text_encoder(captions)        # N embeddings
    M[i][j] = dot( I[i], T[j] )       # N x N similarity matrix

    # maximise the diagonal, minimise everything else
    loss = cross_entropy(M, labels=range(N), axis=0) \
         + cross_entropy(M, labels=range(N), axis=1)
```

The **diagonal** of the matrix is the true pairs; every off-diagonal cell is a
negative example generated for free by the batch. That is why no manual
labelling is needed ? the caption *is* the label.

## 2. Create a dataset classifier from label text

> **"A photo of a {object}"**

Each class name is written into a sentence template and encoded. The classifier's
weights are **sentences**, not learned parameters.

## 3. Zero-shot prediction

```
def classify(image, class_names):
    v = image_encoder(image)
    t = [text_encoder(f"a photo of a {c}") for c in class_names]
    return class_names[argmax([dot(v, ti) for ti in t])]
```

A class never seen in training can be added by **typing its name**.

> [!note] [[winner-take-all]], instance eight ? now over a vocabulary
> An argmax over similarity scores, exactly as in the [[self-organising-map|BMU]],
> [[saliency-model|saliency]] and [[behaviour-coordination|action selection]].
> The competitors here are *words*.

## Why this belongs in a bio-inspired module

It is the module's third answer to the **symbol grounding** question.

| | Mechanism | Grounded by |
|---|---|---|
| [[word2vec]] (L04) | co-occurrence in text | **nothing** ? text only |
| [[embodied-language-representation]] (L04) | words tied to sensorimotor experience | acting in the world |
| **CLIP** (L10) | shared embedding space, contrastive alignment | **paired perception** |

CLIP grounds language in **images** rather than in action, which puts it
between the two: not embodied, but not purely distributional either. The word
*cup* has a meaning that depends on what cups look like.

> [!note] And it is a fusion architecture
> Two modality-specific encoders mapped into one common space ? the *shared
> representation* option from [[fusion-strategies]] (L07), now learned
> contrastively at scale. It is also, structurally, what
> [[cross-modal-stimuli-prediction]] wanted: given one modality, retrieve the
> other.

## Unclear in the source

- The encoders' architectures are not stated.
- No connection is drawn to the module's own grounding discussion.
- CLIP appears on the [[gwr-network|GWR]]/[[openpose]] page with no stated link
  to gesture recognition at all. Presumably it is offered as a route to
  **open-vocabulary** gesture labels, but the source does not say so.

## See also

- [[word2vec]] ? [[embodied-language-representation]] ? [[fusion-strategies]]
