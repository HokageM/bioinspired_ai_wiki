---
title: Genetic neural encoding
type: concept
sources: [L11]
tags: [evolution, neural-networks, representation]
updated: 2026-09-21
---

# Genetic neural encoding

> **What to encode?**
>
> - **Connection weights:** real value for each edge
> - **Parameters of transfer function:** *n* values for each node
> - **Topology:** fixed or under evolutionary control

Three levels, and the third is the one that matters ? it is the question
[[L11-evolutionary-computing]] opens with and that no other lecture in the module
answers.

## Fixed topology: two layouts

> **Alternative representations:**
>
> **Nodes + weight table**
> `| x? y? | x? y? | x? y? | w?? w?? w?? w?? w?? w?? |`
>  ? node 1 ?              ????????? weight table ?????????
>
> **Nodes and their incoming weights combined**
> `| x? y? w?? w?? | ?`
>  ????? node 1 ?????

Same information, different **linearisation** ? and the difference is not
cosmetic:

> [!note] The layout decides what crossover can preserve
> [[recombination|n-point crossover]] has a **positional bias**: it keeps
> together genes that sit close together on the genome. In the second layout, a
> node and all its incoming weights are **contiguous**, so a crossover point
> between nodes transfers a *complete, functional unit* to the offspring. In the
> first layout, a node's parameters and its weights sit in different regions and
> crossover routinely separates them, producing a node whose weights came from a
> different parent.
>
> The lecture presents the two layouts as *"alternative representations"* with no
> comment. Given that it explains positional bias two pages later, the connection
> is available and not drawn. The second layout is clearly the better one, for a
> reason the lecture supplies itself.

## Encoding a topology

```
# the genome must describe a graph, not just its weights
genome = {
  "nodes": [ (params...) for each node ],
  "edges": [ (from, to, weight) for each connection ],
}
```

Variable-length, structured, and no longer a vector ? which is why
[[mutation]] for networks needs special operators (*add/delete nodes*,
*add/delete connections*) and why [[recombination]] becomes
*"destructive"* and demands *"domain knowledge"*. See
[[competing-conventions-problem]] for the deepest reason it is hard.

## See also

- [[neuroevolution]] ? [[candidate-representation]] ?
  [[competing-conventions-problem]] ? [[mutation]]
