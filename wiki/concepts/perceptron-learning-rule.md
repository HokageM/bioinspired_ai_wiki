---
title: Perceptron Learning Rule
type: concept
tags: [learning, supervised, neural-networks]
sources: [L03, L10]
created: 2026-09-20
updated: 2026-09-20
status: solid
---

# Perceptron Learning Rule

The module's first **supervised**, error-driven learning rule (L03, p2):

$$\Delta w_i = \eta\,(t - y)\,x_i$$

where $t$ is the desired output, $y$ the actual output, $(t - y)$ the **error**,
and $\eta$ the learning rate.

## Biological origin

None. This is the point at which the module leaves biology: the rule requires a
**teacher** supplying $t$ for every input, which no synapse has access to. The
lecture frames it plainly — learning means *updating network weights based on an
existing set of input–output pairs* $(\phi(\vec{x}), \vec{x})$.

Compare [[hebbian-learning]], whose update $\Delta w_{ij} = x_j y_i$ needs no
target at all. The two rules differ by exactly one substitution:

```text
Hebb:        dw = eta * y * x        # use the output you produced
Perceptron:  dw = eta * (t - y) * x  # use how wrong it was
```

That substitution is what makes the rule converge instead of self-amplifying —
see [[hebbian-learning]] § Problems.

## Computational form

```text
# perceptron learning
initialise w randomly
eta = learning_rate                        # L03 writes eta = 1/35 (unexplained)

repeat:
    errors = 0
    for each training pair (x, t):

        y = phi(dot(w, x))                 # forward pass
        e = t - y                          # error

        if e != 0:
            for each i:
                w[i] = w[i] + eta * e * x[i]
            errors += 1

until errors == 0                          # guaranteed to terminate IF the data
                                           # is linearly separable — see
                                           # [[perceptron-convergence-theorem]]
```

Behaviour of the update, for a positive input $x_i$:

| Case | $t - y$ | Effect on $w_i$ |
|---|---|---|
| Output too low | $> 0$ | Increase — push the boundary towards firing |
| Output too high | $< 0$ | Decrease |
| Correct | $0$ | No change — correct examples are inert |

The last row is the important one: the rule is *error-driven*, so it stops
moving once it is right. This is why it terminates and [[hebbian-learning]] does
not.

Geometrically (see [[neural-similarity-and-dot-product]]) the update rotates
$\mathbf{w}$ towards misclassified positive examples and away from
misclassified negative ones — i.e. it rotates the separating hyperplane of
[[linear-separability]] until no example is on the wrong side.

## Where it appears in the module

- [[L03-computational-neural-networks]] — page 2. The lecture's example uses a
  **linear activation function**.

## See also

- [[perceptron-convergence-theorem]] — when this is guaranteed to work.
- [[xor-problem]] — when it cannot work at all.
- [[backpropagation]] — the generalisation to multiple layers.
- [[hebbian-learning]], [[stdp]] — the unsupervised alternatives from L02.
- [[learning-paradigms]] — this is the *supervised* box.

## Open questions / gaps

- **$\eta = 1/35$** appears in the margin with no explanation — presumably from
  a worked example that was not transcribed.
- The rule is stated for a single unit; the notes do not say how to apply it to
  the [[multi-layer-perceptron]] introduced on the same page. (It cannot be —
  that is what [[backpropagation]] is for, but the notes do not make the gap
  explicit.)
- No stopping criterion or epoch count is given here; that arrives on page 4 as
  [[batch-vs-online-training]].


## L10 ? a third notation for the same quantity

[[L10-gesture-recognition]]'s recap writes the error as `E_net = ?(y ? ?)?`,
where `y` is the network's output and `?` the target.

| Lecture | Output | Target | Error |
|---|---|---|---|
| L03 | `o` | `t` | `E = ??(t ? o)?` |
| L06 | `y` | `d` | squared difference |
| **L10** | `y` | **`?`** | `E_net = ?(y ? ?)?` |

The overbar is the problem: elsewhere in the module it marks an **average**
(and does so in L05's [[cross-correlation-localisation|correlation]] material).
Here it marks a **target**. Nothing in the source warns of the collision.

Three conventions for one equation across ten lectures is a real cost for a
revision document ? it makes the identity of the quantity harder to see than the
quantity itself.
