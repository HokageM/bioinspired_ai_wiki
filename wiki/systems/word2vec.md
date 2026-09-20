---
type: system
title: Word2Vec (CBOW and skip-gram)
sources: [L04, L10]
tags: [nlp, embedding, unsupervised, statistical-learning]
---

# Word2Vec

L04's *computational model for application*: **language representation with
Word2Vec**.

## The problem it solves

Classical representation of words in NLP uses **atomic words** — one-hot codes:

```
cat  [1, 0, 0]
seed [0, 1, 0]
cake [0, 0, 1]
```

These are orthogonal, so they carry **no similarity information**. You need an
**additional taxonomy** (WordNet and such) to say that *cat* is closer to *dog*
than to *cake*.

Word2Vec replaces this with a learned dense vector:

```
cat  [0.63, 0.12, 0.04, …]
```

- **Word embedding in vector space.**
- **Distributed representation, inherently compositional** — see
  [[word-embedding]] and [[local-vs-distributed-representation]].
- Connectionist model: a **simple single-layer perceptron**.
- **Learn vectors from context or neighbouring words.**

## The two variants

| | CBOW | Skip-gram |
|---|---|---|
| Full name | Continuous Bag of Words | — |
| Direction | context words → centre word | centre word → context words |
| Captures | *"syntactic"* relationships | *"semantic"* relationships |
| Shape | many in, one out (inputs summed at projection) | one in, many out |

**CBOW** — using context words, we can predict the centre word. The word is
presented as a distributed probability vector. Maximise:

```
(1/T) Σ_{t=1..T}  log p( w_t | w_{t−c/2}, …, w_{t−1}, w_{t+1}, …, w_{t+c/2} )
```

**Skip-gram** — with the centre word we can predict context words. *Mirror of
CBOW.* Maximise:

```
(1/T) Σ_{t=1..T}  Σ_{−c/2 ≤ j ≤ c/2, j ≠ 0}  log p( w_{t+j} | w_t )
```

`c` is the context window size; `j ≠ 0` excludes the word itself.

## Pseudocode

```
# ---- shared setup ----
# V     : vocabulary size
# d     : embedding dimension
# c     : context window size
# W_in  : V x d   "input"  embeddings  (the vectors you keep)
# W_out : V x d   "output" embeddings

initialise W_in, W_out randomly

# ---- CBOW: context -> centre ----
for each position t in corpus:
    context <- { w[t+j] : -c/2 <= j <= c/2, j != 0 }
    h       <- mean( W_in[k] for k in context )      # "SUM" at the projection layer
    scores  <- W_out · h
    p       <- softmax(scores)
    grad    <- p - onehot(w[t])                      # maximise log p(w_t | context)
    W_out   <- W_out - eta * outer(grad, h)
    for k in context:
        W_in[k] <- W_in[k] - eta * (W_out^T · grad) / |context|

# ---- skip-gram: centre -> context ----
for each position t in corpus:
    h <- W_in[w[t]]
    for j in -c/2 .. c/2, j != 0:
        scores <- W_out · h
        p      <- softmax(scores)
        grad   <- p - onehot(w[t+j])                 # maximise log p(w_{t+j} | w_t)
        W_out  <- W_out - eta * outer(grad, h)
        W_in[w[t]] <- W_in[w[t]] - eta * (W_out^T · grad)

# the embedding of a word is its row of W_in
```

> [!note] Softmax over the whole vocabulary is impractical
> Real implementations replace it with hierarchical softmax or negative
> sampling. `[external]` The notes give only the objective, not the trick.

## The famous result

```
Queen = King + Woman − Man
```

plus **word clustering**. The notes record this as `eg:`. It is the evidence
that the embedding space has **linear semantic structure** — which is what
"inherently compositional" above is pointing at, and it connects directly to
[[compositionality-of-language]].

## Is it bio-inspired?

Barely, and the lecture does not claim it is. Its only biological
contact points:

- It is a **distributed representation**, like the brain's
  ([[embodied-language-representation]]).
- It is **learned from co-occurrence statistics**, which is a distant relative
  of [[hebbian-learning]] — *things that occur together get linked*. The
  lecture's own summary groups "Hebbian learning, SOM learning, **statistical
  learning**" as one family, which is the closest it comes to saying so.

But it is trained by gradient descent on a supervised-shaped objective
constructed from unlabelled text — i.e. [[self-supervised-learning]], not
anything a neuron does.

## Unclear in the source

- The CBOW diagram shows **SUM** at the projection layer; the pseudocode above
  uses mean. The original paper averages. `[external]`
- The syntactic/semantic split between CBOW and skip-gram is asserted, not
  argued.
- No dimension `d`, no window size `c`, no corpus is given.

## See also

[[word-embedding]] · [[local-vs-distributed-representation]] ·
[[compositionality-of-language]] · [[self-supervised-learning]] · [[gpt]] ·
[[mcculloch-pitts-neuron]] · [[L04-embodied-language-processing]]


## L10 ? a contrastive alternative

[[contrastive-language-image-pretraining|CLIP]] trains a text encoder and an
image encoder so that matching pairs have high similarity and mismatched pairs
low similarity. The result is a word representation whose geometry is shaped by
**images**, not by neighbouring words.

| | word2vec | CLIP |
|---|---|---|
| Signal | co-occurrence within text | image-caption pairs |
| Objective | predict context | contrastive: diagonal up, off-diagonal down |
| Negatives | sampled | **free** ? every other item in the batch |
| Similarity | dot product of embeddings | dot product **across modalities** |

Both end in a **dot product** and an argmax ? see
[[neural-similarity-and-dot-product]] and [[winner-take-all]]. What changes is
where the supervision comes from, and it is the older criticism of word2vec that
CLIP answers: a purely distributional embedding can place *cup* near *mug*
without any sense of what either looks like.

> [!note] Zero-shot classification is a vocabulary-sized competition
> CLIP's classifier weights are **sentences** ? *"a photo of a {class}"* ? so a
> new class is added by typing its name. Compare word2vec, where the vocabulary
> is fixed at training time.
