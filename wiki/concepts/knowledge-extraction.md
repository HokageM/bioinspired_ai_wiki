---
title: Knowledge extraction
type: concept
sources: [L12]
tags: [explainability, representation, methods]
updated: 2026-09-21
---

# Knowledge extraction from neural networks

Getting a readable account of what a trained network has learned. The lecture
organises it by **what you look at**, and the ordering is an argument:

> **Weights represent knowledge at a very detailed level.**
> **Activation values represent more integrated knowledge for particular
> pattern.**
> **Weights ? knowledge statically. Processing is needed to represent knowledge
> more dynamically.**

| Level | Object | Method | Grain |
|---|---|---|---|
| 1 | weights | [[weight-based-transfer]], [[hinton-diagram]] | finest, least meaningful |
| 2 | activations | clustering, dendrograms, PCA | per-pattern |
| 3 | dynamics | [[automata-extraction]], [[preference-moore-machine]] | per-**sequence** |

> [!note] The progression is the module's own history of representation, read backwards
> A weight is a [[local-vs-distributed-representation|distributed]] fragment: it
> means nothing alone. An activation pattern is an actual **representation** of
> an input. A trajectory through activation space is a **computation**.
>
> Each level up integrates over the level below, and asking *"what does this
> network know?"* only becomes answerable at the level where the thing being
> asked about exists. Knowledge is not in the weights any more than a program is
> in a transistor.

## Activation analysis

> **Cluster the activation patterns of all internal representations of
> sequences.**
> **The internal layer represents the learned knowledge about abstract
> categories ? dendrogram.**
> **Using PCA.**

```
def activation_analysis(net, inputs):
    H = [net.hidden(x) for x in inputs]      # internal representations
    dendrogram(hierarchical_cluster(H))      # which inputs are treated alike
    scatter(pca(H, n=2))                     # the layout of the space
```

The dendrogram in the notes separates **VG**, **PG** and **NG** ? verb,
prepositional and noun groups ? into distinct branches, which is the finding:
the network has grouped inputs by **abstract syntactic category** without being
told the categories exist.

> [!note] This is a stronger result than the lecture presents it as
> It is evidence that the internal layer is not merely a convenient
> intermediate but a **representation of a linguistic abstraction**. That is a
> direct answer to the *"function approximation vs cognition"* challenge ? and
> exactly the standard [[ann-brain-correspondence]] calls *"both sides written
> down"*: a structure is claimed and independently measured.
>
> Clustering internal states is also what the module did in L04 with
> [[self-organising-map|SOM]] maps of word meaning. Same method, opposite
> purpose: L04 used the layout as the *result*, L12 uses it as the *explanation*.

## Stepwise dynamic analysis of the learning behaviour

> - **Initial high error equally distributed**
> - **Learning of often-occurring noun groups**
> - **Learning of less often prepositional groups, verb groups later**
> - **Learning of sequential context later**
> - **Learning of exceptions** [last]

A fourth kind of explanation, and the only one in the lecture that is **temporal**
? it explains not what the network knows but the **order in which it came to know
it**.

```
frequent regularities  ?  rarer regularities  ?  context  ?  exceptions
```

> [!note] Frequency first, exceptions last, context in between
> This is the shape of a learning curve driven by gradient descent on a corpus:
> common patterns dominate the error early and get fixed first. It also mirrors
> the order in which children acquire the same structures [external] ? the
> lecture does not make the comparison, and it would have been a much better
> developmental argument than most in the module.
>
> It also explains the **verbose/brittle** trade-off from
> [[neural-symbolic-integration]] from the inside: exceptions are learned last
> and least well, which is exactly where extracted rules will be wrong.

## See also

- [[weight-based-transfer]] ? [[automata-extraction]] ? [[transducer-network]] ?
  [[explainable-ai]]
