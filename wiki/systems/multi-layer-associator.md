---
type: system
title: Multi-Layer Associator
sources: [L04]
tags: [language, hebbian, associative-memory]
---

# Multi-Layer Associator

L04's model *for language processing*: a **computational model for auditory word
recognition and motor control of speech**, built as a chain of associators.

- It is a **multi-layer neural network**.
- It **mimics neuroanatomical connectivity and neurophysiological properties of
  language-related areas.**

That second point is what separates it from a generic [[multi-layer-perceptron]]:
the layers are not chosen for capacity, they are chosen to stand for cortical
areas.

## Architecture

```
        produce  →
  A1 → AB → … → M1
        ←  recognise
```

- **A1** — morpheme / **auditory form**.
- **M1** — **articulatory form**.
- Intermediate layers (`AB`, …) stand for the association cortex between them.
- The chain is **bidirectional**: forward is *production*, backward is
  *recognition*. This mirrors the two pathways of
  [[language-areas-of-the-brain]] — Wernicke→Broca and back.
- Sparseness: **25 × 25 cells each layer**.

## Learning

**Neural net for word learning via action–perception correlation.**

Trained with the Hebbian rule ([[hebbian-learning]]):

```
Δw_ij = α · x_i · x_j
```

No teacher signal. The word form and the articulation are *co-active* during
babbling/imitation, so the correlation between them is what builds the link.
This is the module's cleanest example of Hebb doing real work — contrast
[[backpropagation]], which needs a target.

## Pseudocode

```
# ---- Hebbian association across a layer chain ----
# layers L[0..K], L[0] = A1 (auditory), L[K] = M1 (articulatory)
# W[k] : weights from layer k to layer k+1
# alpha: learning coefficient

for each training episode (auditory_pattern, articulatory_pattern):

    # co-activate both ends of the chain simultaneously
    x[0] <- auditory_pattern
    x[K] <- articulatory_pattern

    # let activation settle through the intermediate layers
    repeat until stable:
        for k in 1 .. K-1:
            x[k] <- activation( W[k-1]^T · x[k-1] + W[k] · x[k+1] )
            x[k] <- k_winners_take_all(x[k])     # enforce sparseness

    # purely local Hebbian update: no error, no backward pass
    for k in 0 .. K-1:
        for i in layer k, j in layer k+1:
            W[k][i][j] <- W[k][i][j] + alpha * x[k][i] * x[k+1][j]

# ---- recognition (auditory -> articulatory) ----
def recognise(auditory_pattern):
    x <- auditory_pattern
    for k in 0 .. K-1:
        x <- k_winners_take_all( activation(W[k]^T · x) )
    return x

# ---- production runs the same chain in the other direction ----
```

> [!warning] Sparseness is mis-stated in the source
> The notes give "**Sparseness: 25 × 25 cells each layer**". 25×25 is the
> *size* of a layer, not its sparseness. Sparseness is the fraction of cells
> active at once. The `k_winners_take_all` step above is the wiki's
> reconstruction of what must have been meant; the notes do not contain it.

> [!warning] Unbounded Hebbian growth
> `Δw = α·x_i·x_j` with no decay, no normalisation and no upper bound grows
> without limit — the same problem flagged on [[hebbian-learning]]. The notes do
> not mention it. Real associator models use weight normalisation or a
> Hebb-with-decay variant `[external]`.

## Relation to other pages

- It is the **bio-shaped** answer to language. [[word2vec]] is the
  **statistics-shaped** answer, and [[gpt]] the **scale-shaped** one. L04
  presents all three without ranking them — see
  [[ann-brain-correspondence]].
- Shares its bidirectional, settle-to-equilibrium flavour with
  [[recurrent-neural-network]], but it has no time series.

## See also

[[hebbian-learning]] · [[language-areas-of-the-brain]] ·
[[embodied-language-representation]] · [[symbol-grounding]] ·
[[multi-layer-perceptron]] · [[L04-embodied-language-processing]]
