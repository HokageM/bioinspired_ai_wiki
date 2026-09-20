---
title: Associative GWR
type: system
sources: [L13]
tags: [continual-learning, self-organisation, supervised, competitive-learning]
updated: 2026-09-21
---

# Associative GWR

> **How does GWR perform classification? ? Associative GWR.**
> **For each neuron, keep a list of the classes of all inputs for which the neuron
> was the BMU.**
> **When a GWR classifies a novel input, determine the BMU and determine the class
> with the most entries in the neuron's list.**

```
def train_label(x, label):
    labels[bmu(x)].append(label)       # annotate, do not change the weights

def classify(x):
    return most_common(labels[bmu(x)])  # majority vote of the winner's history
```

An unsupervised [[gwr-network|GWR]] made into a classifier by attaching a **label
histogram** to every neuron.

## Why this is more interesting than it looks

The weights are still learned **without labels**. Labels never influence where
neurons go or when the network grows ? they only annotate the result. Consequences
the lecture does not draw:

- **The same trained network can be relabelled for a new task** at zero cost.
  The expensive part (the topology) is task-independent.
- **Labels can arrive late, or partially.** Any input whose class is known
  contributes a list entry; the rest still shape the map.
- The classifier is **[[winner-take-all]]** ? the tenth instance in the module.
  One unit decides; its neighbours are ignored, even though they were close
  enough to be neighbours.
- A neuron whose list is evenly split between classes is a **detectable failure**:
  it sits on a class boundary, and the network's own growth rule could be used to
  split it. The lecture does not suggest this.

> [!note] The cheapest possible bridge between learning paradigms
> [[learning-paradigms]] presents unsupervised and supervised learning as separate
> regimes. Associative GWR shows they can be **layered**: unsupervised learning
> builds the representation, supervision reads it out. Compare
> [[self-organising-map|SOM]], where a trained map is typically labelled the same
> way afterwards ? and compare [[knowledge-extraction]] (L12), which is the same
> move again: learn without labels, then attach an interpretation.

## See also

- [[gwr-network]] ? [[gamma-gwr]] ? [[growing-dual-memory]] ?
  [[self-organising-map]] ? [[learning-paradigms]]
