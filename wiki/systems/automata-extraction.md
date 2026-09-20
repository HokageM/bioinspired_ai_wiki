---
title: Activation-based automata extraction
type: system
sources: [L12]
tags: [explainability, recurrent, representation, methods]
updated: 2026-09-21
---

# Activation-based automata extraction

> **Weights represent knowledge at a very detailed level.**
> **Activation values represent more integrated knowledge for particular
> pattern.**
> **Activations of internal representations can be used for automata
> extraction.**
> **RNN transferred to finite state automaton.**
>
> e.g. **if state is `x` and input is `y` then go to state `z`**

The third and deepest level of [[knowledge-extraction]]: not what the network
weighs, nor what it represents, but what it **computes**.

## The idea

A [[simple-recurrent-network|recurrent network]]'s hidden vector is a point in a
continuous space, and it moves as the input arrives. If the points cluster, each
cluster can be called a **state**, and the transitions between clusters are a
**finite state automaton** ? a discrete object with a readable diagram.

```
def extract_automaton(net, sequences):
    states = {}
    transitions = {}
    for seq in sequences:
        h = net.init_state()
        for symbol in seq:
            h_next = net.step(h, symbol)
            s, s_next = quantise(h), quantise(h_next)   # continuous -> discrete
            transitions[(s, symbol)] = s_next
            h = h_next
    return transitions
```

`quantise` is the whole method, and [[preference-moore-machine]] gives the
lecture's version of it: snap each vector to the **nearest corner** of the unit
hypercube.

## Why this is the strongest form of explanation in the lecture

An automaton is not a summary of the network's behaviour ? it is an **executable
model** of it. You can run it, and check whether it agrees with the network on
inputs neither was derived from. Every other method in L12 produces something
you can only look at.

| Method | Output | Can it be tested? |
|---|---|---|
| [[hinton-diagram]] | a picture | no |
| [[weight-based-transfer]] | rules | in principle |
| [[class-activation-map]] | a heatmap | no |
| [[layer-wise-relevance-propagation]] | a heatmap | no |
| **Automata extraction** | **a machine** | **yes ? run both** |

The lecture does not make this point, and it is the answer to its own unaddressed
fidelity problem: an extracted automaton is the one explanation whose
faithfulness is **measurable**.

## The dynamic/static argument

> **Weights ? knowledge statically. Processing is needed to represent knowledge
> more dynamically.**

The justification for the whole progression from weights to activations to
automata. A recurrent network's knowledge is **not in its weights** in any
readable sense; it is in the trajectory the weights induce. Looking at the
parameters of a dynamical system tells you very little about its behaviour,
which is exactly why the [[hinton-diagram]] *"does not show dynamics of recurrent
networks."*

> [!note] It also assumes the network is really a finite automaton
> An RNN has a continuous state space and can in principle occupy infinitely many
> states; quantising asserts that it does not ? that it has settled into a
> **finite** number of clusters. When true, the extraction is faithful. When
> false, states get merged and the automaton is wrong in a way that inspection
> alone will not reveal.
>
> Whether a trained RNN really becomes finite-state is an empirical question
> [external] and the lecture treats it as given.

## See also

- [[preference-moore-machine]] ? [[transducer-network]] ?
  [[knowledge-extraction]] ? [[hinton-diagram]]
