---
title: Developmental and curriculum learning
type: concept
sources: [L13]
tags: [learning, development, continual-learning, biology]
updated: 2026-09-21
---

# Developmental and curriculum learning

The first of L13's four related paradigms.

> - **Critical periods of learning**
> - **Early experiences are the most influential**
> - **Curriculum of increasingly complex tasks**

Sketch: two curves against time ? **complexity rising**, **plasticity falling**.

```
for stage in curriculum:            # easy -> hard
    train_until_competent(stage)
    learning_rate *= decay          # plasticity falls as competence rises
```

## The argument

Order the data rather than sampling it randomly. Learning `A` before `B` can be
faster than learning them together, if `A`'s solution is a usable starting point
for `B` [external]. Nature does this with **critical periods** ? windows of high
plasticity during which particular capacities must be acquired, after which the
system stiffens.

> [!note] The falling-plasticity curve is [[gwr-network|GWR]]'s habituation counter
> `?w_b ? h_b`, and `h_b` decreases with use: a GWR neuron is plastic when new and
> stiff once practised. That is a critical period, implemented, **per neuron**.
>
> The module has therefore built this paradigm already and presents it two pages
> later as a "related" direction. The connection is not made.

> [!warning] It also contradicts the lecture's premise
> [[continual-learning]] requires learning from **"changing input distributions"**
> with **no control over the order** ? tasks arrive as they arrive. A curriculum
> is precisely control over the order, i.e. a designer who knows the task sequence
> in advance. Same assumption the lecture attacks in
> [[continual-language-learning|BERT's fine-tuning recipe]] on the previous page.
>
> The resolution is in the third paradigm: **self-generated** curricula
> ([[intrinsic-motivation]]), where the agent orders its own experience. The
> lecture lists the problem and its answer as two separate bullets.

## See also

- [[intrinsic-motivation]] ? [[transfer-learning]] ? [[continual-learning]] ?
  [[gwr-network]] ? [[synaptic-plasticity]]
