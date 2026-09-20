---
title: Preference Moore machine
type: system
sources: [L12]
tags: [explainability, recurrent, representation]
updated: 2026-09-21
---

# Preference Moore machines and RNNs

The lecture's concrete method for turning a recurrent network into a readable
machine.

> **Output** `Q = [0,1]^m`
> **States** `S = [0,1]^p`
> **Input** `I = [0,1]^n`
> joined by a **preference mapping**
>
> **IF state is `a` and input is `b` THEN go to state `x` and output is `y`**

## The preference mapping

> **For all output vectors and for all concatenations of input and state vector,
> find the closest symbolic representation (closest corner preference vector
> according to some metric, e.g. Euclidean).**

```
def corner_preference(v):
    return tuple(1 if x > 0.5 else 0 for x in v)    # nearest corner of [0,1]^n

def to_moore_machine(net, sequences):
    delta = {}
    for seq in sequences:
        s = net.init_state()
        for x in seq:
            s_next, q = net.step(s, x)
            delta[(corner_preference(s), corner_preference(x))] = (
                 corner_preference(s_next), corner_preference(q))
            s = s_next
    return delta
```

A hidden vector `(0.9, 0.1, 0.8)` becomes the symbol `101`. The continuous state
space `[0,1]^p` has `2^p` corners, and each becomes one **discrete state**.

The worked example in the margin shows transitions between corner symbols
`001 ? 010`, `000 ? ?`, labelled with words (`n/ng`, `v/vg`, `d/ng`) ? the
[[transducer-network]] rendered as a state machine.

> [!note] Why corners, and not clusters
> Corner preference needs no training, no choice of `k`, and no fitted model ?
> it is a **deterministic** function of the vector. That makes the extracted
> machine reproducible, which a clustering-based extraction is not.
>
> The price is that the states are chosen **in advance** rather than found in the
> data. If the network's activations cluster somewhere other than near corners,
> corner preference splits one real state across several symbols or merges
> several into one. It works well when activations **saturate** ? which sigmoid
> units do, pushing values towards 0 and 1 ? and would work badly with ReLU or
> tanh activations. The dependence on the activation function is unmentioned.

## Moore, not Mealy

A **Moore** machine's output depends on the **state** only; a Mealy machine's
depends on state **and** input [external]. The rule as stated ?
*"IF state is `a` and input is `b` THEN go to state `x` and output is `y`"* ?
makes `y` depend on both, which is a **Mealy** transition.

> [!warning] The stated rule does not match the stated machine
> Either the output is a function of the next state `x` (making it genuinely
> Moore, with the rule written loosely), or the machine is a Mealy machine and
> the name is wrong. The diagram's `Q` is drawn as an output of the state, which
> favours the first reading. The source does not disambiguate.

## Flexibility of abstraction level

> **Symbolic interpretation of RNN as transducers:** mapping from vectors to
> symbolic representation ? presentation of all patterns ? computation of next
> corner preference for each output vector and state vector ? computation of
> symbolic transducer ? **flexibility of abstraction level.**

The last phrase is the payoff: coarser quantisation gives fewer states and a
simpler, less faithful machine; finer quantisation gives more states and more
fidelity. **Interpretability and fidelity are dials, and they trade against each
other** ? the precise trade-off the lecture never names, here implemented as a
parameter.

## See also

- [[automata-extraction]] ? [[transducer-network]] ? [[knowledge-extraction]]
