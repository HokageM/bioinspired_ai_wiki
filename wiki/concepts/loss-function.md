---
title: Loss Function
type: concept
tags: [learning, supervised, neural-networks]
sources: [L03]
created: 2026-09-20
updated: 2026-09-20
status: solid
---

# Loss Function

The scalar $E(\vec{y}, \vec{t})$ measuring how far a network's output is from
its target — the quantity [[backpropagation]] differentiates.

## Biological origin

None. A loss function presupposes a target, which presupposes a teacher. See
[[learning-paradigms]] — this object exists only in the supervised box.

## Mean squared error (L03, p3)

$$E(\vec{y}, \vec{t}) = \frac{1}{N}\sum_{i \in N}(t_i - y_i)^2$$

Its derivative with respect to an output, as the lecture gives it:

$$\frac{\partial E}{\partial y_i} = t_i - y_i$$

> [!note] Sign and constant
> Differentiating $\frac{1}{N}\sum(t_i-y_i)^2$ properly gives
> $-\frac{2}{N}(t_i - y_i)$. The lecture drops the $-2/N$, keeping just
> $t_i - y_i$. This is consistent with the module's convention of writing the
> update as $w' = w + \eta\delta(\cdot)$ — a plus sign with $\delta = t - y$
> — which absorbs the negation. The constant is absorbed into $\eta$. Harmless,
> but it explains why the module's formulae look sign-flipped against most
> textbooks.

## Regularised form (L03, p4)

$$E(\vec{y}, \vec{t}) = \frac{1}{N}\sum_{i \in N}(t_i - y_i)^2 + \lambda\sum_j w_j$$

See [[regularisation]] — the penalty term as written is not a standard L1 or L2
norm, which is flagged there.

## Computational form

```text
def mse(y, t):
    return sum((t[i] - y[i])**2 for i in 1..N) / N

def d_mse(y, t, i):
    return t[i] - y[i]              # as L03 writes it: dE/dy_i = t_i - y_i

def regularised(y, t, w, lam):
    return mse(y, t) + lam * sum(w[j] for j in weights)

# role in training: E is what the "average difference between y and t is small
# enough" test on p3 measures, and what [[backpropagation]] differentiates.
```

The loss is what converts the supervised problem into an optimisation problem:
the network is not told what weights to use, only how wrong it is, and the
derivative turns that scalar into a direction for every weight.

## Where it appears in the module

- [[L03-computational-neural-networks]] — page 3 (MSE), page 4 (regularised).

## See also

- [[backpropagation]] — consumes this.
- [[regularisation]] — the penalty term.
- [[overfitting-and-underfitting]] — why a low training loss is not the goal.
- [[autoencoder]] — where the target *is* the input, so the loss measures
  reconstruction quality.
- [[perceptron-learning-rule]] — uses the raw error $(t-y)$ without ever
  naming a loss.

## Open questions / gaps

- **MSE is the only loss given.** Cross-entropy is never mentioned, despite
  classification being the running example ("eg: image classification", p2).
- No discussion of why squared error is chosen.
- The factor $\frac{1}{N}$ is written as averaging over outputs $i \in N$; the
  notes do not distinguish averaging over *output units* from averaging over
  *training samples*.
