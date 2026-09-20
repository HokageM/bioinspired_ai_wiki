---
title: Vanishing Gradient Problem
type: concept
tags: [learning, recurrent, neural-networks]
sources: [L03]
created: 2026-09-20
updated: 2026-09-20
status: solid
---

# Vanishing Gradient Problem

**Gradient becomes small → no update** (L03, p5). The failure mode that makes
[[simple-recurrent-network]]s unable to learn long-range dependencies, and the
problem [[gated-recurrent-network]]s exist to solve.

## Biological origin

None. This is a numerical pathology of [[backpropagation]], and specifically of
backpropagation through time.

## Computational form

Backpropagation computes each layer's error derivative from the next layer's by
the chain rule — a *product* of terms. Over many layers, or many unrolled time
steps, that product decays geometrically.

```text
# the error signal reaching step t-k, unrolled backwards k steps:
#
#   delta[t-k]  =  delta[t] * PRODUCT over k steps of ( w * phi'(a) )
#                             \_______________________/
#                                  k factors multiplied
#
# if each factor has magnitude r:
#     r < 1  ->  r^k -> 0     VANISHING  : early steps get no error signal
#     r > 1  ->  r^k -> inf   EXPLODING  : updates blow up
#
# only r ~ 1 propagates information, and that is a knife edge.

# why r < 1 is the common case:
#   the sigmoid derivative phi'(a) = phi(a)(1 - phi(a)) has a MAXIMUM of 0.25.
#   so |w * phi'(a)| < 1 unless |w| > 4.
#   [external] L03 states the symptom but not this cause.
```

Consequence for sequences: the network can learn dependencies a few steps back,
but the gradient linking an output at step $t$ to an input at step $t-50$ has
been multiplied by ~50 small numbers and is numerically zero. **The network
cannot learn that the two are related** — not because the architecture lacks
the connection, but because the training signal cannot reach it.

## The fix

[[gated-recurrent-network]]s replace the repeated multiplication with an
*additive* cell state: the forget gate can hold a value at ≈1, so the gradient
passes through many steps undiminished. The notes state the outcome —
**LSTM addresses the vanishing gradient problem** (L03, p5) — without the
mechanism.

## Where it appears in the module

- [[L03-computational-neural-networks]] — page 5, as the weakness of the SRN.

## See also

- [[simple-recurrent-network]] — where it bites.
- [[gated-recurrent-network]] — the remedy.
- [[backpropagation]] — the chain-rule product that causes it.
- [[activation-function]] — the saturating sigmoid is the usual culprit; note
  that L02 lists *bounded* and *asymptotic* as virtues of the sigmoid, and here
  those same properties are the problem.

## Open questions / gaps

- **The cause is not explained** — the notes record only "gradient becomes
  small → no update". The chain-rule product above is `[external]`.
- The **exploding** gradient counterpart is not mentioned.
- No mention of ReLU, gradient clipping, or careful initialisation as partial
  remedies.
- The notes do not say *how* LSTM addresses the problem, only that it does.
