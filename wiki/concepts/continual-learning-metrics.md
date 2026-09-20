---
title: Evaluating continual learning
type: concept
sources: [L13]
tags: [continual-learning, evaluation, statistics]
updated: 2026-09-21
---

# Evaluating continual learning

> **Average accuracy:**  `A_k = (1/k) ? ?_{j=1..k} a_kj`
> **Forgetting:**  `f_j^k = max_{m ? {1,?,k?1}} (a_mj) ? a_kj`

where `a_kj` = accuracy on task `j` after the model has been trained through
task `k`.

```
# after finishing task k, evaluate on every task seen so far
A[k]   = mean(acc[k][j] for j in 1..k)
f[j,k] = max(acc[m][j] for m in 1..k-1) - acc[k][j]
```

> [!success] The module's longest-standing gap, closed in the final lecture
> ~~No lecture in this module defines a single evaluation measure. Systems are
> asserted to work; nothing says how anyone would know.~~ L13 writes down two.
>
> Still no *numbers* ? not one result is reported anywhere in thirteen lectures ?
> but there is now a definition of what a result would be.

## Why these two and not accuracy

A single accuracy number cannot express the problem. The whole point is that
performance on task `j` **changes after you stop training on it**, so you need a
matrix `a_kj`, and both metrics are summaries of that matrix:

- `A_k` reads **one row** ? how good is the model right now, across everything.
- `f_j^k` reads **one column** ? how much of task `j`'s peak has been lost.

You need both. A model can keep high average accuracy while quietly destroying its
first task, if the later tasks are easier.

> [!note] `f_j^k` uses the right control, which is not obvious
> Forgetting is measured against **`max_m a_mj`**, the model's own **best past
> performance** on that task ? not against its accuracy immediately after training
> on `j`, and not against a separately trained baseline.
>
> The `max` matters because performance on task `j` can **improve** after training
> on a later related task ([[transfer-learning|backward transfer]]). Using
> `a_jj` as the reference would score that improvement as zero forgetting and then
> miss the subsequent loss from the higher peak. Taking the maximum makes
> forgetting measure *loss from the best the model ever was*, which is the only
> definition that survives positive backward transfer.
>
> A negative `f_j^k` is then meaningful too: the model got **better** at an old
> task by learning a new one.

## What is still missing

- No measure of the **resource** each strategy spends ? yet
  [[continual-learning-strategies|the costs]] (capacity, size, memory) are how the
  four families differ. A method that stores every sample would score perfectly
  on both metrics.
- No measure of **forward transfer** ? does prior learning make the next task
  faster? ? though L13 names forward and backward transfer one page later.
- No baselines: neither joint training (the upper bound) nor naive fine-tuning
  (the lower bound) is mentioned [external].

## See also

- [[continual-learning]] ? [[catastrophic-forgetting]] ?
  [[continual-learning-strategies]] ? [[transfer-learning]]
