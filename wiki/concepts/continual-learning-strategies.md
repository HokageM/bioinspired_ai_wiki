---
title: Continual learning strategies
type: concept
sources: [L13]
tags: [continual-learning, learning, architecture]
updated: 2026-09-21
---

# CL strategies

Four families. The lecture highlights each family's **cost** in orange, and the
costs are the structure of the page.

## 1. Regularisation

> - **Constraints on the update of neural weights by means of additional loss
>   terms**
> - **Habituation of parameters important for previously seen data**
> - **Fixed model capacity**
> - e.g. **[[elastic-weight-consolidation|Elastic Weight Consolidation]]**

```
loss = task_loss(new_task)  +  lambda * sum( importance[w] * (w - w_old)**2 )
#                               ^ pulls important weights back towards their old values
```

Cheapest in memory: nothing is stored but a per-weight importance score. The cost
is **fixed capacity** ? every task competes for the same parameters, and once
enough of them are stiff there is no room left to learn anything.

> [!note] "Habituation of parameters" is the lecture reusing a biological word for a new job
> [[gwr-network|GWR]] uses *habituation* for a firing counter that makes a
> frequently-winning neuron update less. Here it means an importance-weighted
> stiffness on a weight. Same idea ? **use reduces subsequent change** ? applied
> at two different granularities, and the notes use the one word for both without
> comment.

## 2. Dynamic architectures

> - **Apply modular changes to architectural properties in response to novel
>   input**
> - **Introduce task-specific parameters** (e.g. add neurons)
> - **Increasing model capacity and computational cost with further training**
> - e.g. **[[progressive-neural-network|Progressive NN]]**

```
for each new task:
    freeze all existing parameters
    allocate new units / a new column
    connect them to the frozen features (lateral connections)
    train only the new parameters
```

Forgetting is **structurally impossible** ? nothing old is ever written. The cost
is unbounded growth, and the lecture states it: capacity and compute rise with
every task.

This is also the family [[gwr-network|GWR]] belongs to, and the only one the
module built anything in.

## 3. Rehearsal and replay

> - **Rehearsal: explicitly stored training samples.** Periodical retraining on a
>   subset of previously seen data.
> - **Pseudo-rehearsal: artificial samples are generated from the data
>   distribution of prior tasks based on representation learning**
> - **Generative replay: latent representation of the input**
> - **Memory requirements and computational cost grow with #tasks**
> - e.g. **[[carl|CaRL]]**

```
for each batch of the new task:
    old = sample_from_memory()        # stored samples, or generated ones
    train_on(batch + old)             # the old task is back in the gradient
```

Directly attacks the cause identified in [[catastrophic-forgetting]]: it makes
the old task **visible to the optimiser** again. That is why it works, and the
lecture does not say it.

> [!warning] Rehearsal violates continual learning's own definition
> [[continual-learning]] requires **"no access to previously seen training
> samples"**. Storing samples is exactly that access. The lecture lists rehearsal
> as a CL strategy two pages after ruling it out.
>
> The progression *rehearsal ? pseudo-rehearsal ? generative replay* is precisely
> the field fixing this: replace the stored samples with a **model** that can
> regenerate them. The notes give all three in order and never draw the arrow.
> Generative replay also converts the memory cost into a *capacity* cost ? the
> generator must hold every past distribution ? so the resource is moved, not
> removed.

### The biological version

> **Memory replay:** memory reactivation and replay for memory consolidation.
> **Cortico-hippocampal interaction** ? during sleep and awake phase.
> **Disrupting slow wave sleep impairs long-term memory consolidation.**
> **Learn also in the absence of sensory input.**

See [[memory-replay]]. Note the direction of the argument: unlike most
bio-inspiration in this module, the engineering method (replay buffers) was not
derived from the biology ? the biology is offered afterwards as
**post-hoc validation** of a technique invented for computational reasons.

## 4. Hybrid approaches

> **Combination of regularization, structural changes and memory replay.**
> **Dual-memory systems**, e.g. **[[growing-dual-memory|Growing-Dual-Memory]]**.

Since the three costs are in *different currencies* ? capacity, size, memory ?
paying a little of each beats exhausting any one.

## Applied to a network

> a) **Retraining with regularization** ? b) **Training with network expansion** ?
> c) **Selective network retraining and expansion**

Three diagrams over `x(t?1) ? x(t) ? t`: (a) protect what exists, (b) add to it,
(c) both, selectively.

## See also

- [[continual-learning]] ? [[stability-plasticity-dilemma]] ?
  [[memory-replay]] ? [[gwr-network]] ? [[growing-dual-memory]]
