---
title: Multi-Layer Perceptron
type: system
tags: [neural-networks, supervised, foundations]
sources: [L03]
created: 2026-09-20
updated: 2026-09-20
status: solid
---

# Multi-Layer Perceptron

A feed-forward network of [[mcculloch-pitts-neuron]] units arranged in layers.
The answer to the [[xor-problem]].

## Biological origin

Inherited from the single unit — see [[levels-of-abstraction]]. The *layering*
itself is justified in L02 by the claim that development in the brain is **how
neurons are interconnected**, but the specific layered feed-forward topology is
an engineering choice, not an observed one.

## Computational form

The lecture's notation (L03, p2):

$$\vec{x} \to \boxed{l_1} \to \boxed{l_2} \to \boxed{l_3} \to \phi(\vec{x}) = \vec{y}$$

> **The weights in a NN determine the approximated function.** (L03)
> *eg: image classification.*

That sentence is the framing the rest of L03 depends on: training is *function
approximation*, and the weights are the parameters of the approximation.

### Why layers help (L03, p2)

> Each perceptron in the lower layer can separate **linearly**. The output
> neuron in the last layer can separate the input space **non-linearly**.

Each hidden unit contributes one hyperplane ([[linear-separability]]); the
output unit composes them into a region no single hyperplane could describe.

```text
# forward pass — "matrix multiplications for each layer" (L03, p3)
def forward(x, layers):
    a = x
    for layer in layers:
        a = phi(layer.W @ a + layer.b)      # one matmul + nonlinearity per layer
    return a                                # = y

# the nonlinearity is ESSENTIAL:
#   without phi,  W3 @ (W2 @ (W1 @ x))  ==  (W3 @ W2 @ W1) @ x  ==  W' @ x
#   i.e. a stack of linear layers collapses to ONE linear layer, and the
#   network is no better than a single perceptron. See [[activation-function]],
#   where "non-linear" is listed as a required property of the sigmoid.
```

## Training

The [[perceptron-learning-rule]] cannot train this: it needs a target $t$ for
each unit, and no target exists for a *hidden* unit. That is the credit
assignment problem, and [[backpropagation]] is its solution.

Note also that the [[perceptron-convergence-theorem]] does **not** extend here —
there is no guarantee that training an MLP terminates at a correct solution.

## Where it appears in the module

- [[L03-computational-neural-networks]] — page 2 (as the XOR fix), page 3
  (as the thing backpropagation trains).

## See also

- [[xor-problem]] — the motivation.
- [[backpropagation]] — how it is trained.
- [[mcculloch-pitts-neuron]] — the unit.
- [[network-architectures]] — L02's taxonomy; this is "feed-forward multilayer".
- [[recurrent-neural-network]] — what you use when the input is a sequence.
- [[overfitting-and-underfitting]] — the risk that comes with the added capacity.

## Open questions / gaps

- **No weights are given for a working XOR network**, so the resolution is
  argued but not demonstrated.
- The universal approximation theorem is not mentioned, although "the weights
  determine the approximated function" gestures at it.
- No guidance on depth, width, or how to choose them.
- The notes do not state that the hidden nonlinearity is what makes depth
  meaningful; that is reconstructed above.
