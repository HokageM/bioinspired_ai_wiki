---
title: Regularisation
type: concept
tags: [learning, generalisation]
sources: [L03]
created: 2026-09-20
updated: 2026-09-20
status: developing
---

# Regularisation

Adding a penalty on the weights to the [[loss-function]], so that fitting the
data competes against keeping the model simple (L03, p4).

## Biological origin

None stated. (A metabolic-cost argument for penalising large weights would be
natural here, but the notes do not make it.)

## Computational form

The form given in the notes:

$$E(\vec{y}, \vec{t}) = \frac{1}{N}\sum_{i \in N}(t_i - y_i)^2
+ \lambda\sum_j w_j$$

> [!warning] The penalty term as written is not a norm
> $\sum_j w_j$ sums the weights **signed and un-squared**. Such a term is
> minimised by making weights large and *negative*, which is not regularisation
> — it is a bias. The standard forms are
> $\lambda\sum_j |w_j|$ (L1, sparsity) or $\lambda\sum_j w_j^2$ (L2, weight
> decay). The notes almost certainly mean one of these; the missing
> absolute-value bars or square is a transcription slip. `[external]`

```text
# regularised training objective
def total_loss(y, t, w, lam):
    data_term    = mse(y, t)
    penalty_L2   = sum(w[j]**2 for j in weights)     # weight decay
    penalty_L1   = sum(abs(w[j]) for j in weights)   # sparsity
    return data_term + lam * penalty_L2              # pick one

# effect on the update: the penalty contributes its own gradient
#   d/dw of (lam * w^2) = 2 * lam * w
# so every step pulls each weight slightly towards zero:
    w[j] = w[j] - eta * (d_data_term/dw[j] + 2 * lam * w[j])
#                                            ^^^^^^^^^^^^^^ shrinkage

# lam = 0       -> no regularisation, free to overfit
# lam too large -> underfitting: the penalty dominates the data
```

$\lambda$ is therefore the explicit knob on the trade-off named in
[[overfitting-and-underfitting]].

## Where it appears in the module

- [[L03-computational-neural-networks]] — page 4, third of the five remedies for
  overfitting.

## See also

- [[overfitting-and-underfitting]] — the problem.
- [[loss-function]] — what this modifies.
- [[dropout]] — a different, non-penalty regulariser from the same list.
- [[backpropagation]] — the penalty's gradient joins the data gradient here.

## Open questions / gaps

- **The formula as written is wrong** (see warning). Worth confirming against
  the lecture slides which norm was intended.
- $\lambda$ is introduced without discussion of how to choose it.
- L1 vs L2 is not distinguished, and weight decay is not named.
- No mention that regularisation applies to weights but conventionally not to
  biases.
