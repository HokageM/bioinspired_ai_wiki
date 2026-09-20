---
title: Growing When Required (GWR) network
type: system
sources: [L10, L13]
tags: [unsupervised, competitive-learning, plasticity, architecture]
updated: 2026-09-21
---

# Growing When Required (GWR)

> **SOM size is driven by the input and not fixed.**
> **Growth and shrinking: adding or removing nodes.**

The first architecture in the module that **changes its own architecture**.
Everything before it learns by changing weights in a structure fixed in advance.

## The algorithm

Transcribed from the source, which gives it in full ? unusually for this module.

```
initialise two random nodes; connection set C = {}

repeat:
    x(t) = sample from input distribution

    # 1. competition
    d_j = || x(t) - w_j ||                for every node j
    b   = argmin_j d_j                    # BMU
    s   = argmin_{j != b} d_j             # second BMU (SBMU)

    if (b, s) not in C:
        C.add((b, s), age = 0)            # Hebbian: co-winners get linked

    # 2. how well does the BMU already cover this input?
    a(t) = exp( -d_b )                    # activity: 1 when perfect, ? 0 when far

    if a(t) < a_T  and  h_b < h_T:        # poor match AND the BMU is well-trained
        # 3a. GROW
        n = new node
        w_n = (w_b + x(t)) / 2            # placed between winner and input
        C.add((b, n)); C.add((s, n))
        C.remove((b, s))                  # rewire: the new node takes over
    else:
        # 3b. ADAPT (the SOM update, restricted to the BMU and its neighbours)
        for i in {b} + neighbours(b):
            dw_i = eps_i * h_i * ( x(t) - w_i )
            w_i += dw_i
            # firing counter decays with use
            dh_i = tau_i * 1.05 * (1 - h_i) - tau_i
            h_i += dh_i

    # 4. housekeeping
    increment age of all connections of b
    remove connections older than a_max
    remove nodes left with no connections
until stopping criterion
```

## The two thresholds, and why there are two

Growth requires **both** conditions:

| Condition | Meaning | Prevents |
|---|---|---|
| `a(t) < a_T` | the input is **poorly covered** | growing where the map is already good |
| `h_b < h_T` | the winner has **fired often enough** (the counter decays with use) | growing in response to a node that has not had a chance to learn yet |

The firing counter `h` is the clever half. Without it, every novel input would
spawn a node and the network would grow without bound on noise. With it, a node
gets a **grace period**: it must be trained before its failures count as evidence
that a new node is needed.

> [!note] Novelty alone is not a reason to grow ? only *persistent* novelty is
> This is a genuinely good idea and the source does not comment on it.

## Why it answers L10's own critique

[[deep-network-tradeoffs]] objects that deep networks are *"specialist for one
task (adaptivity? feedback?)"*. GWR is the reply: capacity is **allocated where
the data is**, so a new gesture adds nodes rather than requiring a retrain, and
the network never has to be sized in advance.

> [!success] This is almost certainly L08's unnamed architecture
> [[task-inference-network]] (L08) described *"a self-organised network of
> behaviours"* that supports continual learning, and named no algorithm ? the
> wiki flagged it as an open question. GWR is from the same group, does exactly
> that, and solves exactly the stated problem (fixed task representations are
> incompatible with continual learning). Not stated in either lecture; recorded
> as a strong inference, not a fact.

## Relation to the SOM

| | [[self-organising-map|SOM]] | GWR |
|---|---|---|
| Node count | **fixed in advance** | grows and shrinks |
| Topology | fixed grid | learned graph (Hebbian edges) |
| Update | BMU + grid neighbours | BMU + graph neighbours |
| Neighbourhood | decaying kernel over the grid | the connection set |
| Ageing | none | edges age, unused ones die |

The competitive core ? find the BMU, move it towards the input, drag neighbours
with it ? is **unchanged from L04**. What is added is **structural plasticity**.

> [!note] The module's first neurogenesis
> Every learning rule so far has been **synaptic**: change the strength of
> existing connections ([[hebbian-learning]], [[perceptron-learning-rule]],
> [[backpropagation]], SOM). GWR changes **which units and connections exist**.
> Biologically that is adult neurogenesis and synaptic pruning; computationally
> it is a model-selection problem being solved online rather than by a human
> choosing a layer width. Neither framing appears in the source.

## Unclear in the source

- `eps_i`, `tau_i`, `h_T`, `a_T`, `a_max` are never given values or ranges.
- The constant **1.05** in the firing-counter update is unexplained. It makes
  the fixed point `h* > 1`, so `h` can exceed 1 ? no reason is offered.
- `neighbours(b)` is used in the update but the notes only ever *create* edges
  between BMU, SBMU and new nodes; the neighbourhood's extent is not defined.
- No evaluation, dataset or comparison against a SOM.

## See also

- [[gamma-gwr]] ? [[self-organising-map]] ? [[self-organising-map|competitive learning]] ?
  [[task-inference-network]]

## L13 ? the full algorithm

L10 used GWR without specifying it. L13 gives all fifteen steps.

> [!success] The L10 stub is closed
> ~~GWR is named and used but never specified: no growth criterion, no update
> rule, no pruning rule.~~ All three are below, transcribed from the source.

### Setup

> **I.** `A ?` set of **2 neurons**
> **II.** `E ?` set of connections `= ?`
> **III.** At each iteration `t`, receive an input vector `x(t)`

### Per input

> **IV.** Select **BMU** and **SBMU** (second-best matching unit):
> `b = arg min_{j?A} ?x(t) ? w_j??`  ?  `s = arg min_{j?A\\{b}} ?x(t) ? w_j??`
>
> **V.** If there is no connection between `b` and `s`, create it:
> `E = E ? {(b,s)}`
> **VI.** Set the age of this connection to `0`
> **VII.** Calculate current activation: **`a(t) = exp(??x(t) ? w_b??)`**
> ? *how well the BMU fits the input.*

### Case 1 ? grow

> **VIII.** if `(a(t) < a_th)` **and** `(h_b < h_th)` then
> ? *(the BMU does not fit well **and** is not firing often; `h_b` = firing
> counter)*
> **IX.** Add a new neuron `r`: `A = A ? {r}`
> **X.** Create new weight vector: **`w_r = 0.5 ? (w_b + x(t))`**
> **XI.** Connect the new node `r` with `b` and `s`: `E = E ? {(r,b), (r,s)}`
> **XII.** Remove the connection between `b` and `s`: `E = E \\ {(b,s)}`

### Case 2 ? adapt

> **XIII.** else, BMU fits the input well ? update BMU and its neighbours:
> `?w_b = ?_b ? h_b ? (x(t) ? w_b)`
> `?w_i = ?_n ? h_i ? (x(t) ? w_i)`
>
> **XIV.** Increment the age of all edges connected to `b`:
> `age_(b,i) = age_(b,i) + 1`, with `i ? s`
> **XV.** Reduce the firing counters of the BMU and neighbours:
> `?h_b = ?_b ? ? ? (1 ? h_b) ? ?_b`
> `?h_i = ?_n ? ? ? (1 ? h_i) ? ?_n`
> **XVI.** Remove each edge `(i,j)` with `age(i,j) > ?_max`, and **remove isolated
> nodes**

```
A = {n1, n2}; E = {}
for each input x:
    b = argmin_j ||x - w_j||^2                 # BMU
    s = argmin_{j != b} ||x - w_j||^2          # second BMU
    E.add((b,s)); age[(b,s)] = 0
    a = exp(-||x - w_b||^2)                    # activation = fit quality

    if a < a_th and h[b] < h_th:               # poor fit AND habituated
        r = new_neuron(w = 0.5*(w_b + x))      # halfway between BMU and input
        A.add(r); E.add((r,b)); E.add((r,s)); E.remove((b,s))
    else:
        w_b += eps_b * h[b] * (x - w_b)
        for i in neighbours(b):
            w_i += eps_n * h[i] * (x - w_i)
        for i in neighbours(b) if i != s: age[(b,i)] += 1
        h[b] += tau_b*kappa*(1-h[b]) - tau_b   # habituate
        for i in neighbours(b): h[i] += tau_n*kappa*(1-h[i]) - tau_n

    E = {e for e in E if age[e] <= mu_max}     # prune old edges
    A = {n for n in A if has_edges(n)}         # prune isolated nodes
```

### Reading it

**The growth condition is a conjunction, and that is the whole design.** A poorly
fitting input alone does *not* add a neuron ? the winner must **also** be a
well-practised unit (`h_b < h_th`; the counter *decreases* with use). A fresh
neuron is given time to settle before its failures count against it. Without this
the network would add a unit per surprising sample. Neither L10 nor L13's prose
says so; it is only in the `and`.

**The habituation counter appears twice, doing opposite jobs.** In the growth test
it is a *maturity* check; in the update it is a **step size** ? `?w_b ? h_b`, so a
well-used neuron moves less. One quantity implements *"learn fast when new, hold
still when established"*, which is the
[[stability-plasticity-dilemma|stability?plasticity dilemma]] resolved
**per neuron** rather than per network.

**Edge age is a use-it-or-lose-it rule.** Edges to the BMU that were not
co-activated get older; old edges die; isolated nodes die with them. The topology
is therefore continuously re-estimated rather than fixed, which is the difference
from a [[self-organising-map|SOM]]'s rigid grid.

**`w_r = 0.5(w_b + x)`** places the new neuron halfway between the failure and
the input ? a compromise, not the input itself, so one outlier cannot plant a
neuron on top of itself.

> [!note] Growth is the one thing the growth rule cannot control
> `a_th` and `h_th` decide the network's eventual size, and L13 gives **no
> values and no rule for choosing them** ? nor for `?_b`, `?_n`, `?_b`, `?_n`,
> `?`, `?_max`. Eight free constants, all unspecified, which is a lot for the
> module's most fully specified algorithm. See
> [[L13-continual-learning]]'s "Unclear in the source".

## L13 ? why GWR is a continual learner

> - **Expandable / shrinkable networks**
> - **Hebbian-like structural plasticity**
> - **Input-driven self-organization**
> - **Neurogenesis: neural activation, habituation**
>
> **GWR can cope with CL tasks as new neurons can be created to allocate novel
> knowledge.** Large GWRs are expensive ? **prune unused neurons**. Catastrophic
> interference is **still possible** ? **protect knowledge.**

GWR belongs to the **dynamic architecture** family in
[[continual-learning-strategies]], and pays that family's price: it grows, so it
must prune. The last bullet is the honest one ? allocating new units reduces
[[catastrophic-forgetting|interference]] but does not eliminate it, because
Case 2 still moves existing weights.

## L13 ? Associative GWR (classification)

> **How does GWR perform classification? ? Associative GWR.**
> **For each neuron, keep a list of the classes of all inputs for which the neuron
> was the BMU.** When a GWR classifies a novel input, **determine the BMU and
> determine the class with the most entries in the neuron's list.**

```
def train_label(x, label):
    b = bmu(x); labels[b].append(label)
def classify(x):
    return most_common(labels[bmu(x)])
```

An unsupervised network made supervised by bolting a **label histogram** onto
each unit ? the weights are still learned without labels, and the labels only
annotate the result. It is the cheapest possible bridge between
[[learning-paradigms|unsupervised and supervised]] learning, and it means a GWR
can be relabelled for a new task without retraining.
