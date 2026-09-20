---
title: Continual learning
type: concept
sources: [L13]
tags: [continual-learning, learning, foundations, memory]
updated: 2026-09-21
---

# Continual learning (lifelong learning)

> - **Ability to continually acquire, fine-tune and transfer new knowledge and
>   skills over time** (lifelong learning)
> - **Overcome [[catastrophic-forgetting|catastrophic forgetting]] when learning
>   from changing input distributions**
> - **No retraining from scratch and no re-access to previously seen data**
> - **Constrained computational and memory resources**

## Against batch learning

| | **Batch learning** | **Continual learning** |
|---|---|---|
| Tasks | **fixed set** | **dynamic number** |
| Data | all training data available | **no access to previously seen samples** |
| Phases | separate train and test | **no distinction between training and test phases** |
| Adding a task | **prone to catastrophic forgetting or interference** | learn without interfering with existing knowledge |

The diagram: batch learning shows one agent facing *Task A* and *Task B* as two
separate boxes with separate goals; continual learning shows a single agent under
one continuous timeline with tasks arriving along it.

> [!note] Each of the four requirements alone breaks the standard recipe
> Not knowing the number of tasks means you cannot size the output layer.
> No re-access means you cannot shuffle, which
> [[backpropagation|gradient descent]] assumes. No train/test split means you
> cannot early-stop or tune on a validation set. Bounded resources means you
> cannot answer by storing everything. Continual learning is not an added
> difficulty on top of supervised learning; it removes supervised learning's
> preconditions one at a time.

> [!warning] And the module has quietly assumed the opposite for twelve lectures
> Every system in this wiki ? [[perceptron-learning-rule|the perceptron]], [[backpropagation]],
> [[convolutional-network]], [[gpt]], [[evolutionary-algorithm]] ? is trained
> once on a fixed dataset. [[self-organising-map|SOM]] and [[gwr-network|GWR]] are
> the exceptions, and L13 reveals that this is *why* they were taught. The module's
> unsupervised, incrementally-growing networks were the continual-learning thread
> all along, and only the final lecture says so.

## Why the i.i.d. assumption is the real culprit

The lecture says *"changing input distributions"*. Standard training assumes
samples are **independent and identically distributed** ? drawn from one fixed
distribution and seen in random order [external]. A task sequence violates both
halves: consecutive samples are correlated, and the distribution shifts at every
task boundary. Every [[catastrophic-forgetting|forgetting]] result is downstream
of that single violation.

## The biological claim

Humans and animals do this. The lecture's summary attributes it to
**[[hebbian-learning]] and [[complementary-learning-systems|complementary
learning systems theory]]** ? the module's most specific and best-supported
biological claim, since both have actual mechanisms attached and CLS has an
experimental result behind it ([[memory-replay|sleep disruption]]).

## See also

- [[catastrophic-forgetting]] ? [[stability-plasticity-dilemma]] ?
  [[continual-learning-strategies]] ? [[continual-learning-metrics]] ?
  [[growing-dual-memory]] ? [[L13-continual-learning]]
