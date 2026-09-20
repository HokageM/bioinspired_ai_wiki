---
type: concept
title: Word Embedding
sources: [L04]
tags: [nlp, representation, distributed-representation]
---

# Word Embedding

Representing a word as a **dense vector in a continuous space**, learned from
its contexts, instead of as an atomic symbol.

## The contrast L04 draws

**Classical representation of words in NLP: atomic words.**

```
cat   [1, 0, 0]
seed  [0, 1, 0]
cake  [0, 0, 1]
```

⇒ **Need additional taxonomy to define similarity or difference.**

Every pair of one-hot vectors is equidistant and orthogonal. The representation
itself contains nothing about meaning, so similarity has to be bolted on from
outside (a hand-built ontology).

**Word embedding:**

```
cat   [0.63, 0.12, 0.04, …]
```

- **Word embedding in vector space**
- **Distributed representation, inherently compositional**
- **Learn vectors from context or neighbouring words**

Now similarity is just distance, and it falls out of training rather than being
declared.

## Why "inherently compositional"

Because the space has linear structure. L04's example:

```
Queen = King + Woman − Man
```

plus **word clustering**. Vector arithmetic performs analogy, which means
relations like *gender* or *plurality* are directions in the space rather than
discrete features. See [[compositionality-of-language]].

## The connection to L02

This is [[local-vs-distributed-representation]] applied to words. One-hot codes
are the **grandmother cell** of NLP: one unit, one word, no generalisation.
Embeddings are the distributed alternative — each word is a pattern over all
units, each unit participates in many words.

It also makes [[neural-similarity-and-dot-product]] directly relevant: with
embeddings, the dot product between two word vectors *is* a similarity measure,
which is precisely the interpretation L02 gave it for neural activity.

## The distributional hypothesis

The assumption underneath is that *a word is characterised by the company it
keeps* — meaning can be recovered from co-occurrence alone. `[external]` L04
states the method (*learn vectors from context*) without naming the assumption
or questioning it.

It is worth questioning, because it directly contradicts
[[embodied-language-representation]] in the same lecture: if meaning is grounded
in multi-modal perception, then text co-occurrence cannot be sufficient. Both
claims are made on the same day and never compared.

## See also

[[word2vec]] · [[local-vs-distributed-representation]] ·
[[neural-similarity-and-dot-product]] · [[compositionality-of-language]] ·
[[embodied-language-representation]] · [[L04-embodied-language-processing]]
