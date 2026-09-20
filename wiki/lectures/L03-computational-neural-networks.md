---
title: "L03 — Computational Neural Networks"
type: lecture
tags: [neural-networks, learning, supervised, recurrent]
sources: [L03]
source_file: raw/lectures/BioinspiredAIWdh3.pdf
lecture_date: 2024-01-25/26
created: 2026-09-20
updated: 2026-09-20
status: solid
---

# L03 — Computational Neural Networks

## Summary

Where [[L02-spiking-neural-networks]] went *towards* biology, L03 goes the other
way: it takes the biological neuron and strips it down until what remains is
trainable at scale. The lecture is explicit about this as a method — three
**[[levels-of-abstraction]]** (structural, functional, temporal) turn a neuron
into a node, and "all physics and chemistry" into "a few parameters associated
with nodes and arcs".

It then delivers the standard supervised-learning stack that L02 conspicuously
lacked: the [[perceptron-learning-rule]], the
[[perceptron-convergence-theorem]], the [[xor-problem]] and its resolution by
the [[multi-layer-perceptron]], [[backpropagation]], the practical training
issues ([[overfitting-and-underfitting]], [[regularisation]], [[dropout]],
[[batch-vs-online-training]]), then sequences
([[recurrent-neural-network]], [[simple-recurrent-network]],
[[gated-recurrent-network]]), and closes with the three
[[learning-paradigms]] and an [[ann-brain-correspondence]] table.

> [!note] This lecture answers L02's biggest gap
> [[linear-separability]] recorded that L02 established the one-hyperplane limit
> but never stated its consequence. L03 states it directly, as the
> [[xor-problem]], and resolves it. That page has been updated.

## Key ideas

### From biology to nodes (L03, p1)
- **Signal transmission in the brain** — a *wave of voltage change along an
  axon*. The nucleus generates an **action potential (AP)** if the sum of its
  incoming **synaptic potentials (SP)** is strong enough.
- **Human brain** — complex physical and chemical activity to transmit a *single*
  action potential along *one* connection.
- **ANN** — *all physics and chemistry represented by a few parameters
  associated with nodes and arcs*. The compression ratio is the point.
- **[[levels-of-abstraction]]** — structural, functional (activation functions),
  temporal (voltage → activation values).
- **Reset options** for the accumulated potential $V$: never reset; reset when
  above threshold; always reset. (Always-reset is the memoryless
  [[mcculloch-pitts-neuron]]; reset-when-above-threshold is
  [[integrate-and-fire]].)

### Supervised learning
- **[[perceptron-learning-rule]]** — $\Delta w_i = \eta(t - y)x_i$, the first
  error-driven rule in the module.
- **[[perceptron-convergence-theorem]]** — given linearly separable data, the
  perceptron finds a separating hyperplane in finitely many steps.
- **[[xor-problem]]** — OR and AND are learnable, XOR is not.
- **[[multi-layer-perceptron]]** — lower-layer perceptrons each separate
  linearly; the output neuron combines them to separate **non-linearly**.
- **[[backpropagation]]** — the three-step error-derivative chain, plus the
  worked $\delta$ example.
- **[[loss-function]]** — mean squared error, and its regularised form.

### Training in practice
- **[[batch-vs-online-training]]** — incremental/online, batch, mini-batch;
  learning parameters are **learning rate** and **momentum**.
- **[[overfitting-and-underfitting]]** — adapting to noise vs failing to adapt
  to structure.
- **[[regularisation]]**, **[[dropout]]** (≈50% of activations zeroed during
  training), data augmentation, early stopping, more data.
- Tooling: *Weights & Biases* named as a library for experiment tracking,
  visualising training, and analysing model performance.

### Sequences
- **[[recurrent-neural-network]]** — inference on variable-length sequences with
  temporal context; internal state; can behave chaotically or oscillate.
- **[[simple-recurrent-network]]** — backpropagation through time by unrolling;
  *biologically more plausible than an MLP*; prone to the
  [[vanishing-gradient-problem]].
- **[[gated-recurrent-network]]** — input, forget and output gates; LSTM
  addresses the vanishing gradient.

### Framing
- **[[learning-paradigms]]** — supervised, unsupervised, reinforced.
- **[[ann-brain-correspondence]]** — the lecture's own mapping table, including
  *backpropagation ↔ plasticity* and *reward ↔ dopamine*.
- **[[autoencoder]]** — error based on reconstruction quality.
- **[[convolutional-network]]** — named in the summary only.

## New pages created

Concepts: [[levels-of-abstraction]], [[perceptron-learning-rule]],
[[perceptron-convergence-theorem]], [[xor-problem]], [[loss-function]],
[[overfitting-and-underfitting]], [[regularisation]], [[dropout]],
[[batch-vs-online-training]], [[vanishing-gradient-problem]],
[[learning-paradigms]], [[ann-brain-correspondence]]

Systems: [[multi-layer-perceptron]], [[backpropagation]],
[[recurrent-neural-network]], [[simple-recurrent-network]],
[[gated-recurrent-network]], [[autoencoder]], [[convolutional-network]]

## Pages updated

[[linear-separability]] (XOR consequence now sourced), [[mcculloch-pitts-neuron]]
(a learning rule finally exists), [[activation-function]] (ReLU appears),
[[network-architectures]], [[index]], [[overview]]

## Connections

- **Back to L02:** L02 built *up* from biology and ended at [[stdp]], a local
  unsupervised rule. L03 builds *down* from biology and ends at
  [[backpropagation]], a global supervised rule. The tension between these two
  is the most interesting thing in the module so far — see
  [[ann-brain-correspondence]], where the lecture claims
  *backpropagation ↔ plasticity* as if they were the same thing.
- **[[discrete-dynamic-neuron]] (L02) and [[simple-recurrent-network]] (L03)**
  are the same idea — a unit with state — reached from opposite directions.
- **Forward:** [[learning-paradigms]] names reinforcement learning, which the
  module has not yet developed.

## Unclear in the source

- **$\eta = 1/35$** is written beside the perceptron learning rule with no
  explanation. Almost certainly a constant from a worked example that was not
  copied into the notes.
- **Date inconsistency:** page 3 is headed *25.01.2023* while pages 4–6 read
  *25.01.2024* and *26.01.2024*. The 2023 is presumably a slip.
- **The backpropagation worked example is partially illegible.** The network
  ($x_1, x_2 \to f_1 f_2 f_3 \to f_4 f_5 \to f_6 \to y$) and the $\delta$
  recursions are readable, but the bottom-right term is ambiguous between
  $\delta = t - y$ and $(t-y)^2$. Recorded as $t - y$ on [[backpropagation]] for
  consistency with the stated $\partial E/\partial y_i = t_i - y_i$.
- **Sign convention:** the notes write the weight update as
  $w' = w + \eta\,\delta\,\frac{df(e)}{de}\,y$ — gradient *ascent* in form,
  because $\delta$ is defined as $t - y$ rather than $y - t$. Consistent, but
  opposite to the usual textbook sign.
- **[[gated-recurrent-network]] conflates LSTM and GRU.** The notes describe
  input/forget/output gates (LSTM), then append "update gate and reset gate"
  (GRU) without naming GRU or separating the two.
- **[[regularisation]] formula uses $\lambda \sum_j w_j$** — the weights appear
  un-squared and without absolute value. Probably shorthand for an L1 or L2
  penalty; as written it is neither.
- **[[autoencoder]] and [[convolutional-network]]** appear only in the closing
  summary, with one line each.
