---
title: Neural Similarity and the Dot Product
type: concept
tags: [neural-networks, geometry, foundations]
sources: [L02, L05, L06]
created: 2026-09-20
updated: 2026-09-21
status: solid
---

# Neural Similarity and the Dot Product

**The output of a neuron is a measure of similarity between its input pattern
and its pattern of connection weights** (L02, p3).

This is the lecture's answer to "how can a neural unit represent similarity?",
and it is the most conceptually loaded idea in L02: it reinterprets the weighted
sum from an arbitrary aggregation into a *geometric* operation.

## Biological origin

Indirect. The claim is about what the weighted sum *means*, not about a
biological mechanism. The biological reading is that a synapse pattern is a
stored template, and a neuron reports how well the current input matches it.

## The derivation (L02, p3)

The lecture gives it in three steps:

**1. Linear algebra.** The weighted sum is a dot product:
$$y = \phi\left(\sum_i w_i x_i\right), \qquad y = \mathbf{w}\cdot\mathbf{x}$$

**2. Similarity / angle between two vectors.**
$$\cos\vartheta = \frac{\mathbf{w}\cdot\mathbf{x}}{|\mathbf{w}||\mathbf{x}|},
\qquad 0 \le \vartheta \le \pi, \qquad |\mathbf{x}| = \sqrt{\mathbf{x}\mathbf{x}^{T}}$$

**3. Output signal.**
$$\mathbf{w}\cdot\mathbf{x} = |\mathbf{w}||\mathbf{x}|\cos\vartheta$$

So the net activation factorises into **magnitude × alignment**. A neuron fires
strongly when the input is large *and* points the same way as its weight vector.
The weight vector is the pattern the neuron is tuned to.

## Computational form

```text
# a neuron as a template matcher
def net_activation(w, x):
    return dot(w, x)                       # = |w| * |x| * cos(angle)

def similarity(w, x):                      # magnitude-invariant version
    return dot(w, x) / (norm(w) * norm(x)) # = cos(angle), in [-1, 1]

# consequence: to make neuron i respond to pattern p, set w_i proportional to p.
# angle = 0    -> cos = 1  -> maximal activation  (perfect match)
# angle = pi/2 -> cos = 0  -> zero activation     (orthogonal / unrelated)
# angle = pi   -> cos = -1 -> maximal inhibition  (anti-pattern)
```

This is also why [[hebbian-learning]] works at all. The Hebb update
$\Delta w_{ij} = x_j y_i$ pushes $\mathbf{w}$ towards inputs that already excite
the neuron — i.e. it *rotates the weight vector towards the input pattern*,
narrowing the angle, increasing the response next time. Hebbian learning is
template-building, in this geometry.

## Where it appears in the module

- [[L02-spiking-neural-networks]] — page 3, directly after the bias-unit
  simplification.

## See also

- [[mcculloch-pitts-neuron]] — the unit whose behaviour this explains.
- [[linear-separability]] — the same geometry read as a boundary rather than as
  an angle: $\mathbf{w}$ is the normal of the separating hyperplane.
- [[hebbian-learning]] — learning as rotation of $\mathbf{w}$.
- [[local-vs-distributed-representation]] — what the matched templates stand for.
- [[cross-correlation-localisation]] (L05) — the same measure, swept over time
  shifts.

## The same idea in L05

L05's localisation algorithm defines its core operation in exactly these terms:

> **Similarity is the max dot product of shifted 0-padded inputs.**
> **Delay is the time difference at maximal similarity.**

[[cross-correlation-localisation]] computes a dot product between two signals at
every possible offset and takes the largest. It is this page's template-matching
picture with one addition: the template is **slid**, and the winning *shift* is
the answer rather than the winning *score*.

The biological counterpart is the [[jeffress-model]], where a bank of
coincidence detectors evaluates all the shifts in parallel — one neuron per
delay. Each such neuron is doing precisely what this page describes: firing in
proportion to how well its input matches what it is tuned for.

So the L02 claim that *a neuron's weighted sum is an alignment measure* turns
out to be load-bearing. Three lectures later it is the operating principle of a
working robot.

> [!note] The normalisation gap below shows up again
> L05's correlator uses the raw dot product with no normalisation, so shifts
> with more overlap score higher for purely geometric reasons. Same problem,
> same silence.

## L06: the third instance — convolution

The convolutional layer of [[L06-hierarchical-vision]] is a sliding dot product:

```
z_i = Σ_{j=0}^{r−1} w_j · x_{i+j}  =  w · x[i : i+r]
```

Each output is the dot product of a filter with a patch — a **similarity
measurement**. The activation map answers, at every position: *how much does the
image here look like this filter?* A kernel is a template, and convolution is
template matching at every location at once.

That makes the L02 idea appear a third time, and in the same form each time:

| Lecture | Where | Slides over |
|---|---|---|
| L02 | the neuron's weighted sum `w·x` | nothing — one alignment |
| L05 | [[cross-correlation-localisation]] | **time delay** `d` |
| L06 | [[convolutional-network]] | **space** `i` |

L05's cross-correlation and L06's convolution are the **same expression** with a
different name for the shift variable. One slides a template over delays to find
*where a sound came from*; the other slides it over positions to find *where a
feature is*. Neither lecture mentions the other, and the notes never observe
that the operation is the same.

> [!note] The normalisation gap, a third time
> L06's convolution is the raw dot product. No normalisation appears, so a
> bright patch outscores a well-matched dim one for reasons of magnitude alone.
> Same problem in L02, L05 and now L06 — and the same silence.

## Open questions / gaps

- The notes use the raw dot product $\mathbf{w}\cdot\mathbf{x}$ as the "output
  signal", so the response is **not** magnitude-invariant: a large but poorly
  aligned input can outproduce a small well-aligned one. Normalisation is not
  discussed.
- The same symbol $\vartheta$ is used here for the angle and elsewhere on the
  same page for the neuron's threshold. Context distinguishes them, but the
  collision is in the source.
