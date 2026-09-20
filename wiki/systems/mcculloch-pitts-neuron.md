---
title: McCulloch-Pitts Neuron (Rate-Coded Unit)
type: system
tags: [neural-networks, foundations, rate-coding]
sources: [L02, L03]
created: 2026-09-20
updated: 2026-09-20
status: solid
---

# McCulloch-Pitts Neuron (Rate-Coded Unit)

The **artificial rate-coded neuron unit** (L02, pp2–3) — the elementary
computing element of the connectionist branch of [[neural-coding]]. Also called
the perceptron in the notes.

## Biological origin

An abstraction of a neuron: weighted inputs stand for synapses, the sum stands
for integration at the soma, and the threshold stands for the all-or-none
[[action-potential]]. The abstraction discards spike timing entirely, keeping
only activity level — see [[rate-coding]].

The notes draw it beside a dendrite sketch to make the correspondence explicit,
and place it in the lineage: *weight matrix → coding based on activity level →
McCulloch-Pitts / Perceptron → connectionism*.

## Computational form

$$y_i = \phi(A_i) = \phi\left(\sum_j w_{ij}x_j - \theta_i\right)$$

Structure (L02, p2):

```
 x1 --w1--\
 x2 --w2---> [ Σ ] --A_i--> [ φ(A_i) ] --> y_i
 x3 --w3--/                     ↑
                              θ_i
```

$\phi$ is the [[activation-function]]: identity, step, or sigmoid. The step
version is given as: if $\sum \ge \theta$ then $1$, else $0$.

### Threshold → bias unit (L02, p3)

Instead of carrying the threshold $\theta_i$ as a separate term, add an
**additional weighted input $x_0$, always $-1.0$**, with its own weight
$w_{i0}$:

$$y_i = \phi\left(\sum_{j=1}^{N} w_{ij}x_j - \theta_i\right)
\;\longrightarrow\;
y_i = \phi\left(\sum_{j=0}^{N} w_{ij}x_j\right)$$

The note's justification: **easier to express / program**. The threshold becomes
just another weight, so the whole unit is one dot product and learning rules
need no special case for $\theta$.

The worked example in the margin: $x_0 = -1$, $x_1 = 0.7$ with $w_1 = 0.3$,
$x_2 = 0.3$ with $w_2 = 0.2$.

```text
# forward pass, threshold form
def unit(x, w, theta, phi):
    A = sum(w[j] * x[j] for j in 1..N) - theta
    return phi(A)

# forward pass, bias-unit form — identical behaviour, no special case
def unit_bias(x, w, phi):
    x[0] = -1.0                         # always, by convention
    A = sum(w[j] * x[j] for j in 0..N)  # w[0] plays the role of theta
    return phi(A)

# whole layer
def layer(x, W, phi):
    x = [-1.0] + x
    return [phi(dot(W[i], x)) for i in 1..M]
```

With $x_0 = -1$, a *positive* $w_{i0}$ subtracts from the activation — so
$w_{i0}$ is numerically equal to the old $\theta_i$, not to its negative. This
sign convention is the lecture's, and differs from the more common $x_0 = +1$
convention used elsewhere.

## What the unit does

Three readings of the same dot product, all from L02 page 3:

| Reading | Page |
|---|---|
| Measures **similarity** between input pattern and weight pattern | [[neural-similarity-and-dot-product]] |
| **Separates** the input space with a hyperplane | [[linear-separability]] |
| Produces an activity level to be passed on | [[rate-coding]] |

## Limitations

- **Static.** Output is determined by the current input; there is no temporal
  processing (L02, p4). Addressed by [[discrete-dynamic-neuron]].
- **One hyperplane.** A single unit is a linear classifier — see
  [[linear-separability]] for the consequence the notes leave unstated.

## Learning (L03)

L02 gives no learning rule for this unit. L03 supplies one — the
[[perceptron-learning-rule]], $\Delta w_i = \eta(t - y)x_i$ — together with the
[[perceptron-convergence-theorem]] and, via the [[xor-problem]], the limits of
what a single unit can be taught at all.

L03 also reframes the unit through [[levels-of-abstraction]], and notes that the
choice of **reset policy** for the accumulated potential is what separates this
static unit ("always reset") from [[integrate-and-fire]] ("reset when above
threshold").

## Where it appears in the module

- [[L02-spiking-neural-networks]] — pages 2–3, then used as the baseline for
  every later model.
- [[L03-computational-neural-networks]] — pages 1–2, as the unit that gets a
  learning rule and gets stacked into a [[multi-layer-perceptron]].

## See also

- [[neural-coding]] — the branch this belongs to.
- [[spiking-neural-network]] — the alternative branch.
- [[activation-function]], [[network-architectures]]
- [[perceptron-learning-rule]] — how it is trained (L03).
- [[multi-layer-perceptron]] — what you build from it.
- [[hebbian-learning]] — the unsupervised rule applied to these units in L02.

## Open questions / gaps

- ~~No learning rule is given for this unit in L02.~~ **Resolved by L03.**
- McCulloch and Pitts are named only through the model's name — no dates, no
  paper, no biography.
- The relationship between "McCulloch-Pitts" (historically binary-threshold) and
  "perceptron" is not distinguished; the notes treat them as one.
