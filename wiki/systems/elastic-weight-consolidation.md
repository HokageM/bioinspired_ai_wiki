---
title: Elastic Weight Consolidation (EWC)
type: system
sources: [L13]
tags: [continual-learning, learning-rule, methods]
updated: 2026-09-21
status: stub-by-source
---

# Elastic Weight Consolidation

> e.g.: **Elastic Weight Consolidation**

Named once, as the example of the
[[continual-learning-strategies|regularisation]] family. `status: stub-by-source`
? no mechanism, no loss term, no reference.

## What the surrounding text implies

The family is described as *"constraints on the update of neural weights by means
of additional loss terms"* with *"habituation of parameters important for
previously seen data"*, so EWC must:

```
loss = task_loss(new_task) + lambda * sum( importance[w] * (w - w_old)**2 )
```

and the open question the notes leave is **where `importance[w]` comes from** ?
which is the entire content of the method. *Elastic* presumably names the
quadratic pull back towards `w_old`, a spring whose stiffness is the importance.

`fixed model capacity` is its stated cost: nothing is added, so tasks compete for
the same weights and the network eventually stiffens everywhere.

## See also

- [[continual-learning-strategies]] ? [[progressive-neural-network]] ?
  [[catastrophic-forgetting]]
