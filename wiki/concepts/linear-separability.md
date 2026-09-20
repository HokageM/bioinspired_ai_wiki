---
title: Linear Separability
type: concept
tags: [neural-networks, geometry, foundations]
sources: [L02, L03]
created: 2026-09-20
updated: 2026-09-20
status: solid
---

# Linear Separability

**A neuron divides the input space into two regions: one where $A \ge 0$ and one
where $A < 0$** (L02, p3).

This is the lecture's answer to "how can a neural unit separate input patterns?"
— the complement to [[neural-similarity-and-dot-product]]. Same algebra, read as
a boundary instead of an angle.

## Biological origin

None claimed. This is the mathematics of what the threshold in an
[[action-potential]] does when carried into a model: a threshold on a weighted
sum *is* a hyperplane.

## The derivation (L02, p3)

For two inputs, the boundary is where the net activation is exactly zero:

$$w_1x_1 + w_2x_2 - \vartheta = 0
\iff
x_2 = \frac{\vartheta}{w_2} - \frac{w_1}{w_2}x_1$$

which is a straight line in the $(x_1, x_2)$ plane with slope $-w_1/w_2$ and
intercept $\vartheta/w_2$. The lecture sketches both cases:

| Case | Effect |
|---|---|
| $\vartheta > 0$ | Line is **offset** from the origin |
| $\vartheta = 0$ | Line passes **through** the origin |

So the weights set the *orientation* of the boundary and the threshold sets its
*offset*. This is precisely why absorbing the threshold into a bias unit (see
[[mcculloch-pitts-neuron]]) loses nothing — the bias weight is the offset.

The weight vector $\mathbf{w}$ is the **normal** to this boundary, which is the
link back to [[neural-similarity-and-dot-product]]: maximal similarity is the
direction perpendicular to the dividing line.

## Computational form

```text
# classification by a single unit
def classify(w, x, theta):
    A = dot(w, x) - theta
    return 1 if A >= 0 else 0          # the two regions

# the boundary itself, in 2D, as the lecture writes it
def boundary_x2(x1, w1, w2, theta):
    return theta / w2 - (w1 / w2) * x1

# geometry:
#   w          -> normal vector; sets the ORIENTATION of the boundary
#   theta      -> sets the OFFSET from the origin
#   theta = 0  -> boundary passes through the origin
```

In $N$ dimensions the line becomes a hyperplane; the form is unchanged.

## The consequence (L03, p2)

L02 establishes the one-hyperplane limit but stops there. **L03 states the
consequence:** a single perceptron can learn OR and AND but **cannot learn
XOR**, because XOR's true cases lie on one diagonal and its false cases on the
other, and no straight line separates diagonals. See [[xor-problem]].

Worse, by the [[perceptron-convergence-theorem]] the failure is not a poor
answer but **non-termination** — the [[perceptron-learning-rule]] keeps
updating forever, because some example is always misclassified.

The fix is to stack units: *each perceptron in the lower layer can separate
linearly; the output neuron in the last layer can separate the input space
non-linearly* (L03). This is the motivation for the
[[multi-layer-perceptron]] — and it is exactly what L02's page 4 architecture
list presents without explanation.

## Where it appears in the module

- [[L02-spiking-neural-networks]] — page 3, the geometry.
- [[L03-computational-neural-networks]] — page 2, the consequence and the fix.

## See also

- [[neural-similarity-and-dot-product]] — the dual reading of the same equation.
- [[xor-problem]] — the canonical non-separable case.
- [[mcculloch-pitts-neuron]] — the unit; the bias trick is this page's
  $\vartheta$ term.
- [[multi-layer-perceptron]] — the resolution.
- [[network-architectures]] — multilayer architectures exist because one
  hyperplane is not enough.
- [[activation-function]] — the boundary sits where $\phi$'s argument crosses 0,
  for any monotonic $\phi$.

## Open questions / gaps

- ~~The XOR problem is not mentioned.~~ **Resolved by L03** — see
  [[xor-problem]]. L02 leaves the gap; L03 fills it.
- L02 gives no learning rule for *finding* a separating boundary. **Resolved by
  L03** — see [[perceptron-learning-rule]].
- Neither lecture gives concrete weights for a working XOR network.
