---
title: Progressive Neural Network
type: system
sources: [L13]
tags: [continual-learning, architecture, methods]
updated: 2026-09-21
status: stub-by-source
---

# Progressive Neural Network

> e.g.: **Progressive NN**

Named once, as the example of the
[[continual-learning-strategies|dynamic architecture]] family.
`status: stub-by-source` ? no mechanism given.

## What the surrounding text implies

The family *"applies modular changes to architectural properties in response to
novel input"* and *"introduces task-specific parameters (e.g. add neurons)"*, at
the cost of *"increasing model capacity and computational cost with further
training"*. So:

```
for each new task:
    freeze every existing parameter        # forgetting becomes impossible
    add new units / a new column
    connect them to the frozen features
    train only the new parameters
```

Freezing makes [[catastrophic-forgetting|forgetting]] **structurally impossible**
rather than merely unlikely ? which is why this family is the one counterexample
to L13's claim that forgetting *"affects all connectionist models"*.

The price is that the model only ever grows, and `n` tasks cost `n` times the
parameters.

## See also

- [[continual-learning-strategies]] ? [[elastic-weight-consolidation]] ?
  [[gwr-network]] ? [[catastrophic-forgetting]]
