---
title: Backpropagation
type: system
tags: [learning, supervised, neural-networks]
sources: [L03, L08, L11]
created: 2026-09-20
updated: 2026-09-20
status: solid
---

# Backpropagation

The algorithm that trains a [[multi-layer-perceptron]] by propagating error
derivatives backwards through the layers (L03, p3).

## Biological origin

**None given, and this matters.** The module asserts
*backpropagation ↔ plasticity* in [[ann-brain-correspondence]], but
backpropagation requires each weight to know the errors and weights of every
downstream layer — information no synapse has. See that page for the full
tension with L02's local rules.

## The training loop (L03, p3)

> - Randomly initialise connection weights
> - Start loop:
>   - For each training pair of inputs $\vec{x}$ and output targets $\vec{t}$:
>     - **Forward pass:** compute $\phi(\vec{x}) = \vec{y}$ performing matrix
>       multiplications for each layer
>     - **Update:** weights based on $\vec{y}$ and $\vec{t}$
>   - If average difference between $\vec{y}$ and $\vec{t}$ is small enough:
>     end loop and return weights

## The three steps (L03, p3)

1. **Compute $E(\vec{y}, \vec{t})$** — e.g. mean squared error,
   $\frac{1}{N}\sum_{i\in N}(t_i - y_i)^2$. See [[loss-function]].
2. **Compute error derivatives in each hidden layer from error derivatives in
   the subsequent layer.** At the output: $\frac{\partial E}{\partial y_i} = t_i - y_i$.
3. **Use error derivatives for activations to get error derivatives for the
   weights.**

Step 2 is the recursion — each layer's error is built from the *next* layer's,
which is why the pass runs backwards. Step 3 is the conversion from "how wrong
is this activation" to "how should this weight change".

## The worked example (L03, p3)

Network: $x_1, x_2 \to f_1, f_2, f_3 \to f_4, f_5 \to f_6 \to y$.

Forward:
$$y_1 = f_1\big(w_{(x_1)1}x_1 + w_{(x_2)1}x_2\big), \qquad
y = f_6\big(w_{46}y_4 + w_{56}y_5\big)$$

Backward — the $\delta$ recursion:
$$\delta = t - y, \qquad \delta_5 = w_{56}\,\delta, \qquad
\delta_3 = w_{34}\,\delta_4 + w_{35}\,\delta_5$$

Weight updates:
$$w'_{46} = w_{46} + \eta\,\delta\,\frac{df_6(e)}{de}\,y_4$$
$$w'_{(x_1)3} = w_{(x_1)3} + \eta\,\delta_3\,\frac{df_3(e)}{de}\,x_1$$

Read the pattern: **update = learning rate × (error arriving at this unit) ×
(slope of its activation function) × (input along this weight)**. Every update
in the network has that shape; only the three factors change.

```text
# backpropagation, one training pair
def backprop(x, t, layers, eta):

    # --- 1. FORWARD, keeping the intermediate values ---
    a = [x]
    e = []                                   # pre-activation "net input"
    for L in layers:
        e.append(L.W @ a[-1])
        a.append(phi(e[-1]))
    y = a[-1]

    # --- 2. ERROR AT THE OUTPUT ---
    delta = [t - y]                          # dE/dy, per L03's convention

    # --- 3. PROPAGATE BACKWARDS ---
    for k from last_layer down to 1:
        # each unit's error = weighted sum of the errors it fed into
        delta[k-1] = transpose(layers[k].W) @ delta[k]
        #   scalar form, exactly as L03 writes it:
        #       delta_3 = w_34 * delta_4 + w_35 * delta_5

    # --- 4. CONVERT TO WEIGHT UPDATES ---
    for k in all layers:
        for each weight w[i][j] in layer k:
            w[i][j] += eta * delta[k][i] * dphi(e[k][i]) * a[k-1][j]
            #          ^eta  ^error       ^slope          ^input

    return layers
```

> [!note] Sign convention
> The module writes `w += eta * delta * ...` with $\delta = t - y$. Most
> textbooks write `w -= eta * delta * ...` with $\delta = y - t$. These are the
> same algorithm. See [[loss-function]] for where the sign is absorbed.

## Where it appears in the module

- [[L03-computational-neural-networks]] — page 3.

## See also

- [[multi-layer-perceptron]] — what it trains.
- [[loss-function]] — what it differentiates.
- [[batch-vs-online-training]] — when the update is applied.
- [[vanishing-gradient-problem]] — how the backward pass fails over depth/time.
- [[ann-brain-correspondence]] — the disputed biological claim.
- [[hebbian-learning]], [[stdp]] — the local alternatives.

## Open questions / gaps

- **The activation derivative $\frac{df(e)}{de}$ is used but never given** for
  any specific $\phi$. [[activation-function]] lists no derivatives either, so
  the module never closes this.
- The bottom-right of the worked example is ambiguous between $\delta = t - y$
  and $(t-y)^2$; recorded as $t-y$ for consistency with step 2.
- No derivation — the chain rule is never invoked by name.
- "Average difference small enough" is the stopping criterion, with no
  threshold given and no validation set (see [[overfitting-and-underfitting]]).


## The same surface, over space instead of weights (L08)

[[potential-field-navigation]] in [[L08-behaviour-based-robotics]] is gradient
descent with the domain changed:

| | L03 (here) | L08 |
|---|---|---|
| Surface over | weights | physical position |
| Objective | error | `U_total = U_attraction + U_repulsion` |
| Step taken by | backpropagation | driving |
| Local minimum means | a poor model | a **stalled robot** |
| Escape heuristic | noise, restarts | **noise schema** |

The pathology and the remedy are identical, and in L08 the failure is physically
visible: the robot stops between an obstacle and its goal because two vectors
cancel. See [[local-minima-problem]].

Neither lecture references the other, though the second is the first made
tangible.


## L11 ? what gradients cannot reach

[[L11-evolutionary-computing]] opens by naming three things backpropagation
cannot optimise:

> **But: how to select necessary topology, weights and efficient learning
> parameters?**

Gradient descent optimises the **weights** of a network whose **topology** and
**hyperparameters** a human fixed beforehand. Two of the three are discrete or
govern the procedure itself, so no derivative exists with respect to them.

| | Requires | Can optimise |
|---|---|---|
| Backpropagation | differentiable loss and activations | continuous weights |
| [[evolutionary-algorithm|EA]] | **a scalar fitness value** | weights, topology, hyperparameters, anything encodable |

The cost of dropping the gradient is severe and the lecture states it:
*"long runtimes compared to search algorithms"*, *"no guarantee in optimal
solution"* ([[genetic-inverse-kinematics]]). A gradient is a very large amount of
information about which way to move; an EA discards it and pays in evaluations.

> [!note] The sharpest sentence about gradients in the module comes from the evolution lecture
> [[mutation]]: *"Using only standard mutation without recombination, evolution
> becomes a **parallel gradient search**."*
>
> Small random steps, keep what improves, repeated across many independent
> points ? that is stochastic hill climbing, and it is what an EA degenerates
> into once [[recombination]] stops working. Which is exactly the situation
> [[competing-conventions-problem]] predicts for evolved neural networks.

> [!note] And the two can be combined
> [[neuroevolution]]: *"can be combined with learning."* Evolution searches the
> discrete structure; backpropagation refines the weights inside it. Each applied
> where it is competent ? see [[genotype-and-phenotype]] for why the learned
> improvement is not inherited and steers evolution anyway.
