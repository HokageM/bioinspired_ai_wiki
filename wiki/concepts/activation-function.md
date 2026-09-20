---
title: Activation Function
type: concept
tags: [neural-networks, foundations]
sources: [L02, L03, L10]
created: 2026-09-20
updated: 2026-09-20
status: solid
---

# Activation Function

The function $\phi$ applied to a unit's net activation $A_i$ to produce its
output, $y_i = \phi(A_i)$.

## Biological origin

It stands in for the neuron's all-or-none firing decision. The step function is
the literal version of the [[action-potential]] threshold; the sigmoid is its
smooth relaxation, which is what makes [[rate-coding]] workable.

## The functions given (L02, pp2–3)

- **Identity**
- **Step** — the notes give it as: if $\sum \ge \theta$ then $1$, else $0$.
- **Sigmoid**

### Properties of the sigmoid (L02)

The lecture lists five, and they are the reason it is the default choice:

| Property | Why it matters |
|---|---|
| Continuous | Small input change ⇒ small output change |
| Non-linear | Without it, stacked layers collapse to one linear map |
| Monotonic | Preserves the ordering of activations |
| Bounded | Output cannot run away |
| Asymptotic | Saturates smoothly rather than clipping |

$$\phi(A_i) = \frac{1}{1 + e^{-kA_i}}$$

where $k$ controls the steepness.

> [!warning] Probable error in the source
> The notes write $\phi(A_i) = \frac{1}{1+e^{-kA_i}} = \tanh(kA_i)$.
> **These are not equal.** The logistic function is bounded in $(0,1)$ and has
> $\phi(0) = 0.5$; $\tanh$ is bounded in $(-1,1)$ with $\tanh(0) = 0$. The true
> relation is $\tanh(z) = 2\sigma(2z) - 1$. The lecture almost certainly listed
> both as examples of sigmoid-shaped functions and the "=" is a note-taking
> compression. Flagged on [[L02-spiking-neural-networks]].

## Computational form

```text
identity(A)       = A
step(A, theta)    = 1 if A >= theta else 0
sigmoid(A, k)     = 1 / (1 + exp(-k * A))         # bounded (0, 1)
tanh(A, k)        = (exp(k*A) - exp(-k*A))
                  / (exp(k*A) + exp(-k*A))        # bounded (-1, 1)

# as k -> infinity, sigmoid(A, k) -> step(A, 0):
# the smooth version becomes the all-or-none version.
```

That limit is the conceptual point: the sigmoid is a *differentiable stand-in*
for the biological threshold, which is precisely the trade that
[[rate-coding]] makes and [[spiking-neural-network]]s refuse.

## Additions from L03

L03 adds two functions to the list and one piece of terminology:

- **ReLU** — named on page 1 alongside the sigmoid as an example of $\sigma(V)$.
  No definition or properties are given.
- **tanh** — used as the example activation in the perceptron diagram (p1) and
  in the [[gated-recurrent-network]] cell (p5). Note that L02 wrongly equates
  tanh with the logistic sigmoid (see warning above); L03 simply uses it.
- **Terminology** (L03, p1): the weighted sum is the **transfer function**, its
  result is the **net input**, and $\phi$ applied to it yields the
  **activation**.

```text
relu(A) = max(0, A)      # [external] definition — L03 names ReLU only
# unbounded above, so it does NOT saturate, and its derivative is 1 for A > 0.
# that is precisely why it mitigates the [[vanishing-gradient-problem]]:
# repeated multiplication by 1 does not decay. L03 does not make this link.
```

> [!note] A tension worth holding
> L02 lists **bounded** and **asymptotic** as virtues of the sigmoid. L03
> introduces the [[vanishing-gradient-problem]], whose usual cause is exactly
> that boundedness — a saturating function has a near-zero derivative, and the
> sigmoid's derivative never exceeds 0.25. The same property is a virtue in one
> lecture and the root of a failure in the next. Neither lecture connects them.

## Where it appears in the module

- [[L02-spiking-neural-networks]] — pages 2–3, as part of the artificial
  rate-coded unit.
- [[L03-computational-neural-networks]] — page 1 (ReLU, tanh, terminology).
- Reused unchanged in [[discrete-dynamic-neuron]] and
  [[continuous-dynamic-neuron]], where it is applied to the *decayed* activation.

## See also

- [[mcculloch-pitts-neuron]] — where $\phi$ sits in the unit.
- [[linear-separability]] — the decision boundary is where $\phi$'s argument
  crosses zero, whatever $\phi$ is.
- [[local-vs-distributed-representation]] — softmax appears there as an
  output-layer function, a sixth $\phi$ not listed here.
- [[vanishing-gradient-problem]] — caused by saturating $\phi$.
- [[gated-recurrent-network]] — gates are sigmoids because the sigmoid is
  bounded in $(0,1)$ and so reads as a proportion.

## Open questions / gaps

- **No derivative is given for any function**, in either lecture — yet
  [[backpropagation]] (L03) uses $\frac{df(e)}{de}$ throughout. The module never
  closes this.
- ReLU is named but not defined.
- The role of $k$ is not discussed beyond appearing in the formula.


## L10 ? the derivative, finally

[[L10-gesture-recognition]] recaps the neural-network basics before introducing
[[multichannel-cnn|MCCNN]], and in doing so supplies the piece the module had
withheld since L03:

> Error: `E_net = ? (y ? ?)?`
> Sigmoid: `f(x) = 1 / (1 + e^(?x))`
> **`f'(x) = f(x)?(1 ? f(x))`**

> [!success] Open thread closed
> ~~The module uses gradient descent from L03 onward but never states the
> derivative of any activation function, so the chain rule is never completable
> from the notes alone.~~ L10 gives it for the sigmoid.

```
# now fully derivable from the module's own material
delta_out = (y - target) * f(net) * (1 - f(net))
dW = -eta * delta_out * x
```

Note what the identity buys: `f'` is expressible **in terms of `f` itself**, so
the forward pass's output can be cached and reused in the backward pass with no
extra exponentials. That is the practical reason the sigmoid was standard, and
the source does not mention it.

Still missing: the derivative of `tanh`, of ReLU, and of the softmax ? and with
them any account of why ReLU displaced the sigmoid, despite ReLU being listed
here since L03. The saturation argument (`f' ? 0` at both tails, so gradients
vanish in deep stacks) follows directly from the identity just given and is not
drawn.

> [!note] A third notation convention for the same error
> `E_net = ?(y ? ?)?` here; L03 wrote `E = ??(t ? o)?`; L06 used yet another
> pairing. The bar in `?` denotes the target, having denoted a **mean** elsewhere
> in the module. See [[perceptron-learning-rule]].
