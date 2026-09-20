---
title: Synaptic Plasticity
type: concept
tags: [neuroscience, learning, plasticity]
sources: [L02, L13]
created: 2026-09-20
updated: 2026-09-20
status: solid
---

# Synaptic Plasticity

**The ability of synapses to strengthen or weaken over time** (L02, p7). In this
module it is the mechanism that makes learning physically possible.

## Biological origin

The lecture's chain of definitions (L02, p7):

- Activation correlations strengthen synaptic connections
  → this **constitutes brain plasticity**.
- **Plasticity** — a *measure for complex task solving capabilities*.
- **Synaptic plasticity** — the ability of synapses to strengthen or weaken over
  time.

The middle claim is the unusual one and worth noting: the lecture treats
plasticity not merely as a mechanism but as a *measure of capability*. More
plastic ⇒ more capable of solving complex tasks.

- **Learning is experience-dependent modification of connection weights** (L02).

The structural picture the notes draw:

```
  x_j  ──────── synapse w_ij ────────▶  y_i
 pre-synaptic                      post-synaptic
    neuron                             neuron
```

## Computational form

Plasticity is the general schema; a *learning rule* is a specific choice of
$\Delta w$.

```text
# the schema every learning rule in this module instantiates
initialise w_ij in [0, 1]                      # L02 gives this range

repeat:                                        # "repeated presentation of
    for each pattern p in training_patterns:   #  input (training) patterns"
        present(p)
        y = forward(p)
        for each synapse (i, j):
            dw = RULE(x_j, y_i, timing)        # <- the only thing that varies
            w_ij = w_ij + eta * dw             # modification after each
                                               # presentation step = LEARNING
until training_criterion_met
```

The rules the module supplies for `RULE`:

| Rule | `RULE(...)` | Weights can decrease? |
|---|---|---|
| [[hebbian-learning]] | $x_j y_i$ | **No** |
| [[stdp]] | function of $t_{post} - t_{pre}$ | **Yes** |

## Training protocol (L02, p7)

- **Initial weight** $\in [0,1]$.
- Training is **repeated presentation of input (training) patterns** until a
  **training criterion** is met.
- **Modification of weights after each presentation step** — this is what the
  notes circle and label *learning*.

## Where it appears in the module

- [[L02-spiking-neural-networks]] — page 7, framing both learning rules.

## See also

- [[hebbian-learning]] — the first rule, and its failure modes.
- [[stdp]] — the timing-sensitive successor that adds weakening.
- [[donald-hebb]] — who did not assume weakening existed.
- [[neural-similarity-and-dot-product]] — what changing $\mathbf{w}$ means
  geometrically.

## Open questions / gaps

- The **training criterion** is never specified — no error measure exists in the
  module yet, since [[hebbian-learning]] is unsupervised.
- The learning rate $\eta$ is introduced but never discussed (no schedule, no
  guidance on magnitude).
- Why the initial weight range is $[0,1]$ rather than symmetric around zero is
  not explained; given Hebb's rule cannot produce negative weights, it may
  simply be consistent with that.

## L13 — plasticity as a scheduled quantity

L13 makes plasticity a **variable to be controlled** rather than a property to be
described:

- the [[stability-plasticity-dilemma]] — plasticity traded against retention;
- [[developmental-and-curriculum-learning|critical periods]] — plasticity falling
  over time as task complexity rises;
- [[gwr-network|GWR's habituation counter]] — plasticity falling **per neuron**
  with use, since `Δw_b ∝ h_b`;
- [[continual-learning-strategies|regularisation]] — plasticity reduced
  **per weight** in proportion to importance.

> [!note] Four mechanisms, one idea, at four granularities
> *Reduce plasticity where learning has already succeeded.* Global (curricula),
> per neuron (habituation), per weight (EWC), per task (freezing). The module
> presents them on four separate pages as unrelated techniques.
