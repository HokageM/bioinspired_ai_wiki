---
title: Recurrent transducer network
type: system
sources: [L12]
tags: [recurrent, language, explainability, architecture]
updated: 2026-09-21
---

# Recurrent transducer networks

> **Knowledge extraction from transducer networks.**
> **Recurrent transducer networks for syntactic phrase assignment.**
> **What is trained in examples and what is to be learned is the mapping between
> basic and more abstract syntactic categories.**

Marginal gloss: *"Umwandler mit Zustand"* ? **a converter with state**. That is
the definition: a transducer reads a sequence and **emits** a sequence, carrying
state between steps. Unlike a classifier it produces an output per input, and
unlike a plain [[simple-recurrent-network|recurrent network]] the output is the
point rather than the final state.

## The task

```
Input  (basic categories):   noun, verb, adjective, adverb,
                             preposition, pronoun, determiner
                                        ?
                              ?????????????????????
                              ?  hidden + context ?
                              ?????????????????????
                                        ?
Output (abstract categories): noun group, verb group, prepositional group
```

The example in the margin: `l (n ? ng)`, `though (v ? vg)`, `in (r ? pg)`,
`the (d ? pg)`, `next (j ? pg)`, `week (n ? pg)` ? each word's basic tag mapped
to the phrase it belongs to.

> [!note] Why this task needs state, and why that makes it a good demonstration
> *The* is a determiner in every sentence. Whether it belongs to a **noun group**
> or a **prepositional group** depends on whether a preposition came before it.
> The mapping is therefore not a function of the current input at all ? it
> requires the context vector.
>
> So the network's knowledge **cannot** be read off input-to-output weights, and
> a [[hinton-diagram|static weight picture]] must fail by construction. The
> lecture chose a task that motivates its own methodological progression, and
> does not point this out.

## Why it is the right object to explain

The lecture demonstrates every extraction method on this one system, and the task
has three properties that make it unusually legible:

1. **The abstract categories are known in advance**, so clustering the hidden
   layer can be *checked* against linguistics rather than merely described. That
   is what makes the dendrogram result in [[knowledge-extraction]] meaningful.
2. **The input alphabet is small and discrete**, so
   [[preference-moore-machine|corner preference]] mapping to symbols is natural.
3. **It is genuinely sequential**, so [[automata-extraction]] has dynamics to
   find.

> [!note] Chosen for explainability, not performance
> Nothing is claimed about how well this network parses. It is a **model
> organism** ? small, discrete, with a known ground truth ? which is the correct
> methodology for studying interpretability and is never stated as such.

## How to interpret an RNN

> - **Cluster analysis, activation values, weight interpretation**, e.g.
>   [[hinton-diagram|Hinton diagram]]
> - **Stepwise dynamic analysis** of the learning behaviour over time
> - **Symbolic interpretation of NN as transducers**

The three levels of [[knowledge-extraction]], listed in one place.

## See also

- [[knowledge-extraction]] ? [[automata-extraction]] ?
  [[preference-moore-machine]] ? [[simple-recurrent-network]]
