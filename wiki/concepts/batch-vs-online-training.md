---
title: Batch vs Online Training
type: concept
tags: [learning, neural-networks]
sources: [L03]
created: 2026-09-20
updated: 2026-09-20
status: solid
---

# Batch vs Online Training

**How to update the weights?** — the scheduling question, distinct from the
question of *what* the update is (L03, p4).

## Biological origin

Not discussed, though it is a live point: biological learning is necessarily
**online** — an organism cannot accumulate gradients over a dataset before
changing a synapse. Both [[hebbian-learning]] and [[stdp]] are online rules.
Batch training is therefore a biologically implausible convenience, which the
lecture does not remark on.

## The three schedules (L03, p4)

| Schedule | Update after | Notes |
|---|---|---|
| **Incremental / Online** | each training sample | |
| **Batch** | end of each epoch | *epoch ends when all training samples seen* |
| **Mini-batch** | end of each epoch | *epoch ends when a subset of training data seen* |

**Learning parameters:** *learning rate* and *momentum* (L03).

## Computational form

```text
# ONLINE — update immediately, once per sample
for epoch in 1 .. E:
    shuffle(D)
    for (x, t) in D:
        g = gradient(x, t)
        w = w - eta * g                    # N updates per epoch; noisy path

# BATCH — accumulate over the whole dataset, then one update
for epoch in 1 .. E:
    g_total = 0
    for (x, t) in D:
        g_total += gradient(x, t)
    w = w - eta * g_total / len(D)         # 1 update per epoch; smooth path

# MINI-BATCH — the compromise, and what is used in practice
for epoch in 1 .. E:
    shuffle(D)
    for batch in chunks(D, size=B):
        g = mean(gradient(x, t) for (x, t) in batch)
        w = w - eta * g                    # len(D)/B updates per epoch
```

### Momentum

```text
# momentum: carry a fraction of the previous update forward
v = 0
for each update:
    g = gradient(...)
    v = alpha * v + eta * g                # alpha ~ 0.9 [external]
    w = w - v
# damps oscillation across narrow valleys and accelerates along consistent
# directions. L03 names momentum as a learning parameter but does not define it.
```

The trade: online updates are noisy but frequent and can escape shallow local
minima; batch updates are accurate but slow and prone to settling. Mini-batch
takes most of the benefit of both, which is why it is the default.

## Where it appears in the module

- [[L03-computational-neural-networks]] — page 4, under "training issues".

## See also

- [[backpropagation]] — what computes the gradient being scheduled.
- [[loss-function]] — what the gradient is of.
- [[overfitting-and-underfitting]] — the other half of the training-issues page.
- [[hebbian-learning]], [[stdp]] — inherently online, for biological reasons.

## Open questions / gaps

- **Momentum is named but never defined**; the formula above is `[external]`.
- No guidance on batch size, learning rate magnitude, or schedules.
- "Stochastic gradient descent" is never used as a term, despite being what the
  online and mini-batch rows describe.
- The definition of *epoch* differs between the batch and mini-batch rows in the
  notes, which is how the lecture distinguishes them — but it makes "epoch"
  mean two different things.
