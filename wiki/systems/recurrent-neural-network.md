---
title: Recurrent Neural Network
type: system
tags: [recurrent, neural-networks, dynamics]
sources: [L03]
created: 2026-09-20
updated: 2026-09-20
status: solid
---

# Recurrent Neural Network

> **How NNs perform inference on variable-length sequences with temporal
> context.** (L03, p5)

## Biological origin

Recurrence is pervasive in real neural circuits, and L02 already built the
single-unit version — [[discrete-dynamic-neuron]], a unit with a self-weight
$\mu_i$ and a delay. L03 reaches the same structure from the engineering side,
motivated by sequence data rather than by membrane dynamics. The two pages
describe the same idea; it is worth reading them together.

## Properties (L03, p5)

**Recurrent network with cycles:**

- **The network has internal state: it can remember past states.**
- **Natural modelling of sequential data** — the network can operate over
  *sequences of vectors*, not just a fixed-length window.
- **It can behave chaotically or oscillate** (*eg: good for weather*).

The second point is the practical one. A [[multi-layer-perceptron]] has a fixed
input width, so handling a sequence means choosing a window size in advance and
padding or truncating. An RNN consumes one element at a time and carries state,
so length is not an architectural parameter.

The third is unusual to state and worth keeping: a recurrent system is a
*dynamical* system, so it inherits dynamical behaviours — fixed points, limit
cycles, chaos. That is a capability for modelling oscillatory processes and a
liability for stability.

## Computational form

```text
# RNN over a sequence of arbitrary length T
h = h_0                                   # internal state — the whole point
outputs = []

for t in 1 .. T:                          # T need not be known in advance
    h = phi( W_x @ x[t] + W_h @ h + b )   # state depends on itself
    outputs.append( W_y @ h )

return outputs

# compare [[multi-layer-perceptron]]:
#     y = forward(x)            # x must have FIXED width; no state
# compare [[discrete-dynamic-neuron]] (L02):
#     a = mu * a + sum_j w_ij * x_j(n)    # same self-dependence, one unit,
#                                         # scalar mu instead of a matrix W_h
```

$W_h$ is the matrix generalisation of L02's scalar $\mu_i$, and it is applied
once per time step — which is exactly the repeated multiplication that causes
the [[vanishing-gradient-problem]].

## Where it appears in the module

- [[L03-computational-neural-networks]] — page 5.
- Named as one of the two key structures in [[ann-brain-correspondence]].

## See also

- [[simple-recurrent-network]] — the concrete architecture and its training.
- [[gated-recurrent-network]] — the fix for long-range dependencies.
- [[discrete-dynamic-neuron]], [[continuous-dynamic-neuron]] — L02's route to
  the same idea.
- [[network-architectures]] — L02's taxonomy; "recurrent" is one of the four.
- [[temporal-coding]] — the other way the module handles time, via spikes
  rather than state.

## Open questions / gaps

- The chaos/oscillation remark is given with one example ("good for weather")
  and no analysis.
- No equations are given for the general RNN — the notes go straight from the
  motivation to the SRN diagram.
- Bidirectional RNNs, sequence-to-sequence models and attention are not
  mentioned.
