---
title: BERT
type: system
sources: [L13]
tags: [nlp, language, transformer, self-supervised]
updated: 2026-09-21
status: stub-by-source
---

# BERT ? Bidirectional Encoder Representations from Transformers

> **State of the art (SotA): transformer-based language models**, e.g.
> **Bidirectional Encoder Representations from Transformers (BERT)**.
> **Step 1: unsupervised pretraining. Step 2: supervised fine-tuning.**

`status: stub-by-source` ? the acronym is expanded and the two-step recipe given.
Nothing else: no attention mechanism, no masked-language-model objective, no
architecture, no description of what *bidirectional* means or why it matters.

## What the notes do establish

The **two-step recipe** is the point being made, because it is what
[[continual-language-learning|CLL]] criticises on the same page:

```
step 1:  pretrain on a large unlabelled corpus     # unsupervised / self-supervised
step 2:  fine-tune on the labelled task            # supervised
```

> [!note] The recipe is what the lecture is arguing against
> *"Knowledge about all tasks must be present prior to designing the model"* is a
> criticism **of this recipe**. BERT appears as SotA and as the counterexample at
> once: the best available system, and structurally non-continual.

> [!warning] "Unsupervised" is the wrong word
> Pretraining on raw text is **self-supervised** ? labels are generated from the
> data itself (predict the masked word). The module makes this same conflation for
> [[word2vec]] (L03) and the distinction matters, because self-supervision is
> what makes the pretraining scale.

> [!warning] "Transformer" is never defined in thirteen lectures
> It appears in [[gpt]] (L03), [[hubert]] (L09),
> [[contrastive-language-image-pretraining|CLIP]] (L10) and here ? four systems
> across four lectures, all described as transformer-based, with no lecture
> stating what a transformer is. **Attention**, its central mechanism, is defined
> in L09 for the *visual* sense only and never connected.
>
> This is the module's largest undefined dependency, and it is on the systems it
> calls state of the art.

## See also

- [[gpt]] ? [[continual-language-learning]] ? [[word2vec]] ? [[hubert]]
