---
title: Autoencoder
type: system
tags: [neural-networks, unsupervised, representation]
sources: [L03]
created: 2026-09-20
updated: 2026-09-20
status: stub
---

# Autoencoder

> **Error based on reconstruction quality.** (L03, p6)

A network that encodes its input into a narrower internal representation and
then decodes it back, trained to make the output match the input.

## Biological origin

Not claimed. The connection the module *could* have made but does not: an
autoencoder's bottleneck forces a compressed
[[local-vs-distributed-representation]] — it must discover features, because it
has fewer units than inputs.

## Computational form

The lecture gives only a diagram — a wide input layer, a narrow middle, a wide
output — labelled **Encode** and **Decode**.

```text
# autoencoder
z = encode(x)                  # bottleneck: dim(z) << dim(x)
x_hat = decode(z)              # reconstruction

loss = mse(x_hat, x)           # <<< the target IS the input

# training is ordinary [[backpropagation]] on that loss.
#
# why it is UNSUPERVISED: no external teacher supplies a target. The network
# manufactures its own from the data. See [[learning-paradigms]] — this is the
# standard example of self-supervision, though L03 does not use that word.
#
# why the bottleneck matters: without it, the network learns the identity
# function and nothing is compressed. dim(z) < dim(x) forces it to find
# structure. [external] — L03 draws the narrow middle but does not say why.
```

## Where it appears in the module

- [[L03-computational-neural-networks]] — page 6, one line in the closing
  summary.

## See also

- [[learning-paradigms]] — sits in the unsupervised box.
- [[loss-function]] — reconstruction error is an ordinary MSE with $t = x$.
- [[local-vs-distributed-representation]] — what the bottleneck code is.
- [[backpropagation]] — how it is trained.

## Open questions / gaps

- **One line and one diagram in the source.** No equations, no architecture
  details, no application.
- Denoising, sparse and variational autoencoders are not mentioned.
- The purpose is never stated — dimensionality reduction, pretraining and
  generation are all left implicit.
- The bottleneck's necessity is not explained.
