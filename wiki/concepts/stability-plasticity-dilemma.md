---
title: The stability?plasticity dilemma
type: concept
sources: [L13]
tags: [continual-learning, learning, plasticity, foundations]
updated: 2026-09-21
---

# The stability?plasticity dilemma

> **Stability**
> - preserve and consolidate structured knowledge
> - learn in a slow, sustainable manner
> - **generalization**
>
> **Plasticity**
> - adapt quickly to changes in the environment
> - efficiently learn from novel sensory input
> - **specialization**

A single parameter ? *how much does a weight move per sample* ? controls both,
in opposite directions. Turn it down and the model cannot learn the new task;
turn it up and it destroys the old one.

```
learning_rate -> 0   : perfect stability, no learning        (frozen)
learning_rate -> 1   : perfect plasticity, no memory         (catastrophic forgetting)
```

> [!note] Every continual-learning strategy is a way of making the dilemma local
> The dilemma is only unavoidable if plasticity is a **global scalar**. Each
> strategy in [[continual-learning-strategies]] is an attempt to make it a
> **per-parameter** quantity:
>
> | Strategy | What it makes plastic vs stable |
> |---|---|
> | **Regularisation** | weights *important to old tasks* become stiff; the rest stay free |
> | **Dynamic architecture** | old units frozen; **new units** carry all the plasticity |
> | **Replay** | keeps the old task present in the gradient, so stability is *earned* rather than imposed |
>
> Stated that way, the "dilemma" is really an artefact of using one learning rate
> for a whole network. The lecture never says this, but the three solutions only
> make sense read this way.

## The module's sixth architectural axis

| Axis | From |
|---|---|
| learned ? specified | L05 |
| fast ? contextual | L07 |
| modelless ? modelled | L08 |
| stimulus-driven ? goal-driven | L09 |
| exploration ? exploitation | L11 |
| **stability ? plasticity** | **L13** |

And it is the closest sibling of L11's **exploration/exploitation**: both are
one-dimensional trade-offs between *using what you have* and *getting something
new*, both are usually controlled by a single scalar, and both are solved in
practice by making that scalar adaptive rather than by finding the right value.

## The generalisation/specialisation pairing

The notes attach **generalization** to stability and **specialization** to
plasticity. This is a second claim smuggled into the first, and it is not obvious:
a slowly-learned model is *assumed* to have extracted what is common across tasks,
while a rapidly-adapting one has fitted the present one. It is the
the bias–variance|bias?variance]] distinction under different names
[external], and the lecture asserts the mapping without argument.

## See also

- [[continual-learning]] ? [[catastrophic-forgetting]] ?
  [[continual-learning-strategies]] ? [[synaptic-plasticity]]
