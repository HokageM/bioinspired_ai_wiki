---
title: Weight-based transfer (rule extraction)
type: system
sources: [L12]
tags: [explainability, representation, methods]
updated: 2026-09-21
---

# Weight-based transfer

> **Connectionist weights contain the knowledge.**
> **Problem: distributed representations are difficult to understand and
> modify.**
> **Transfer for weights into symbolic rules.**
> **Often used for feedforward NN.**
> **Grouping, elimination and clustering of weights; N-of-M rules.**

## The method

```
def extract_rules(net):
    for unit in net.units:
        w = incoming_weights(unit)
        w = eliminate(w, threshold)      # drop the negligible ones
        groups = cluster(w)              # tie similar weights together
        yield n_of_m_rule(unit, groups)  # "if >= N of these M inputs, fire"
```

The example given:

```
Feedforward NN            Symbolic rules
      (a)                 a :  -b, c
     /   \                b :  -d, e, f
   (b)   (c)              c :  -f, g
   /|\   /|\
 (d)(e)(f)(g)(h)
```

Each hidden unit becomes a rule whose body is the units feeding it, with signs
from the weights ? negative weights giving negated literals.

## Why the three steps are necessary

**Elimination** ? a unit has weights from everything; most are near zero and
carry no information. Dropping them is what makes a rule short enough to read.

**Clustering** ? weights of similar magnitude are treated as equivalent, so
`0.81` and `0.79` become one condition rather than two. This is what converts a
continuous weight vector into a **discrete** rule, and it is where the
information is lost.

**N-of-M** ? the form a threshold unit naturally takes. A unit firing when a
weighted sum exceeds a threshold is, after elimination and clustering,
approximately *"at least N of these M inputs are active"*. That is the correct
target form, and the lecture names it without defining it.

> [!warning] Every step is lossy, and the lecture does not say what is lost
> The extracted rules are a **second model** that approximates the first. If they
> reproduced the network exactly, the network would be redundant; since they do
> not, the explanation is to some degree false.
>
> How false is the **fidelity** question, and it is the central problem of rule
> extraction [external]. No fidelity measure, error bound or evaluation appears
> anywhere in the lecture ? consistent with its total absence of numbers, but
> unusually consequential here, since an unfaithful explanation is worse than
> none.

> [!note] Threshold units make this work; modern networks do not have them
> The N-of-M form comes from a unit that fires or does not. A ReLU or softmax
> layer has no threshold to read off, and a
> [[convolutional-network|convolutional]] layer's weights are shared across
> positions so a rule over *inputs* is not well defined. The technique is
> described for *"feedforward NN"* of the kind in L03 ? which is presumably why
> the lecture moves on to [[class-activation-map]] and
> [[layer-wise-relevance-propagation]] for deep networks.

## See also

- [[hinton-diagram]] ? [[knowledge-extraction]] ? [[symbolic-ai]] ?
  [[local-vs-distributed-representation]]
