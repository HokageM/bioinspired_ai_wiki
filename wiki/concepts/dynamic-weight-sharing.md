---
type: concept
tags: [learning-rule, convolution, plausibility, hebbian]
sources: [L06]
status: solid
---

# Dynamic weight sharing

The **second** and more interesting of [[L06-hierarchical-vision]]'s two
mechanisms for bridging the plausibility gap identified in [[weight-sharing]].
Where [[data-augmentation]] attacks the problem from the *data* side, this
attacks it from the *architecture and learning rule* side.

## The mechanism

- Done by adding **lateral connectivity** to a [[locally-connected-network]] and
  allowing learning via **Hebbian plasticity**.
- This helps **equalise the weights** of the laterally connected neurons ⇒
  `w_1 ≈ w_4`.
- **Lateral connections are added between every `k`-th neuron** in the input and
  output layer.
- **A sleep phase is introduced every training iteration**, where the weights of
  the laterally connected neurons `i` are updated by the "Hebbian rule":

```
Δw_i  ∝  −( z_i − (1/N) Σ_{j=1}^{N} z_j ) · x  −  γ ( w_i − w_i^init )
              └──── deviation from population mean ────┘   └─ anchor ─┘
```

- ⇒ **stochastic gradient descent on the sum `(z_i − z_j)²`**

## Why it works

**The gradient claim in the notes is correct.** Take the objective

```
E = ½ Σ_i ( z_i − z̄ )²,      z̄ = (1/N) Σ_j z_j
```

Since `z_i = w_i · x`, we have `∂z_i/∂w_i = x`, so (neglecting `z̄`'s own
`O(1/N)` dependence on `w_i`)

```
∂E/∂w_i = ( z_i − z̄ ) · x        ⇒   Δw_i = −η ( z_i − z̄ ) x
```

which is the first term exactly. And since
`Σ_i (z_i − z̄)² = (1/2N) Σ_{i,j} (z_i − z_j)²`, minimising the variance *is*
minimising the pairwise sum the notes write. The annotation checks out.

**What the anchor term is for.** The variance objective alone has a trivial
global minimum: set every `w_i = 0`, then every `z_i = 0`, every unit agrees
perfectly, and the network computes nothing. The `−γ(w_i − w_i^init)` term
prevents this collapse by pulling weights back toward a non-degenerate
reference. `γ` sets the balance between *agreeing* and *staying useful*.

## The significance

This is the closest the module has come to answering its own central question.
It is a rule that is:

- **local** — unit `i` needs only its own activation `z_i`, its input `x`, and a
  population average available through lateral connections. No global error
  signal, no backward pass, no information teleported between distant synapses.
- **functional** — it provably descends a global objective, and that objective
  is precisely the constraint `w_i = w_j` that [[weight-sharing]] imposes by
  fiat.

That combination — *local mechanism, global effect, and the equivalence written
down* — is exactly the standard proposed on [[ann-brain-correspondence]] after
L05's [[jeffress-model]]. L06 meets it. This is the second correspondence in the
module that survives scrutiny.

> [!warning] It is called Hebbian and it is not
> Hebb's rule is **correlational**: `Δw ∝ x · y`, strengthening a synapse when
> pre- and post-synaptic activity coincide. This rule is `Δw ∝ −(z_i − z̄) x`
> — an **error-correcting** rule, where the "error" is a unit's deviation from
> its neighbours. It is *anti*-Hebbian in sign: a unit firing above the
> population average has its weights **decreased**.
>
> This is the **third** time the module has labelled a non-Hebbian local rule
> "Hebbian" — after the [[self-organising-map]] update and the
> [[multi-layer-associator]] in L04. The pattern is now firm enough to state as
> a finding: **the module uses "Hebbian" to mean "local", not "correlational".**
> See [[hebbian-learning]].

## Hebb finally gets a decay term

[[hebbian-learning]] has carried an unresolved complaint since L02: pure Hebb
has no remedy for self-amplification, because `Δw ∝ x·y` can only ever increase
weights for correlated activity, so they grow without bound.

`−γ(w_i − w_i^init)` is exactly such a remedy, and it is the **first time in six
lectures** that the module writes one down. It is a weight decay toward a
reference rather than toward zero — compare the `λ Σ w` penalty in
[[regularisation]] from L03, which decays toward zero (and which the notes wrote
incorrectly, un-squared and signed).

## Pseudocode

```
# One training iteration = an awake phase then a sleep phase.

function train_iteration(net, x, target, eta, gamma, k):

    # ---- awake phase: ordinary task learning, weights stay private ----
    z    = [ dot(net.w[i], patch(x, i)) for i in positions ]
    loss = criterion(z, target)
    net.w = net.w - eta * gradient(loss)

    w_init = copy(net.w)          # reference for the sleep phase anchor

    # ---- sleep phase: equalise laterally connected weights ----
    for i in positions:
        group = lateral_neighbours(i, k)      # every k-th neuron
        z_bar = mean( dot(net.w[j], patch(x, j)) for j in group )
        z_i   = dot(net.w[i], patch(x, i))

        net.w[i] -= eta * ( (z_i - z_bar) * patch(x, i)
                            + gamma * (net.w[i] - w_init[i]) )
```

```
# What the sleep phase converges to, in the limit gamma -> 0:
#     all w[i] in a lateral group become equal
#     => the locally connected net has become convolutional
#     => but it was never told to; it got there by a local rule.
```

## Unclear in the source

- **What is `w_i^init`?** Two readings. The marginal annotation — an arrow
  labelled *start sleep phase* pointing at `w_i^init` — suggests it means **the
  weights at the start of the current sleep phase**, i.e. whatever the awake
  phase just produced. The alternative reading is the weights at the
  *initialisation of training*. The first is far more sensible: it lets the
  sleep phase equalise weights **without undoing the task learning that just
  happened**, which makes the sleep metaphor coherent. The pseudocode above
  takes that reading.
- **Why a "sleep phase"?** The term is introduced without motivation.
  `[external]` The analogy is presumably to memory consolidation during sleep —
  an offline phase, with no new input, that reorganises what was learned while
  awake. The notes never say this.
- **What is `k`?** Never given a value. Nor is it explained why lateral
  connections should skip neurons (*every k-th*) rather than connect all of
  them within a group. Possibly a cost-saving approximation, possibly a
  reflection of real cortical lateral connectivity. Unstated.
- **The sign convention conflicts with the module's own.** Here a leading minus
  makes it descent. Elsewhere the module writes `w ← w + η·δ·(df/de)·y` with
  `δ = t − y` — plus sign, error folded in. Two conventions now coexist. See
  [[loss-function]].
- **No results.** Unlike [[data-augmentation]], which is honestly reported as
  *only small performance improvement*, no performance claim is made for dynamic
  weight sharing at all. Whether it closes the gap is not stated.
- **How lateral connections carry `z̄`** is not described. Computing a mean
  across a group is itself a non-trivial neural operation.

## Related

[[weight-sharing]] · [[data-augmentation]] · [[locally-connected-network]] ·
[[convolutional-network]] · [[hebbian-learning]] · [[synaptic-plasticity]] ·
[[ann-brain-correspondence]] · [[regularisation]] · [[self-organising-map]] ·
[[backpropagation]] · [[L06-hierarchical-vision]]
