---
title: Hebbian Learning (Hebb's Rule)
type: concept
tags: [learning, plasticity, unsupervised]
sources: [L02, L04, L06, L13]
created: 2026-09-20
updated: 2026-09-21
status: solid
---

# Hebbian Learning (Hebb's Rule)

> **"Cells that fire together, wire together."** (L02, p7)

i.e. **activation correlations strengthen synaptic connections**. The lecture
calls it *the simplest and most general form of learning*.

## Biological origin

Proposed by [[donald-hebb]] as the mechanism constituting brain plasticity. It
is **unsupervised** — there is no target, no error, no teacher. The only signal
is the coincidence of pre- and postsynaptic activity.

## Computational form

For a synapse $w_{ij}$ from presynaptic neuron $x_j$ to postsynaptic neuron
$y_i$ (L02, p7):

$$\Delta w_{ij} = x_j y_i$$

$$w_{ij}^{t} = w_{ij}^{t-1} + \eta\,\Delta w_{ij}$$

where $\eta$ is the learning rate.

```text
# Hebbian learning, full training loop
initialise w[i][j] in [0, 1]                  # L02's stated range
eta = learning_rate

repeat:
    for each pattern x in training_patterns:

        # 1. forward pass (see [[mcculloch-pitts-neuron]])
        for each unit i:
            y[i] = phi(sum over j of w[i][j] * x[j] - theta[i])

        # 2. Hebbian update — purely local: needs only x[j] and y[i]
        for each synapse (i, j):
            dw = x[j] * y[i]                  # correlation of the two ends
            w[i][j] = w[i][j] + eta * dw      # can only ever INCREASE
                                              # (for non-negative x, y)
until training_criterion_met
```

The locality is the appeal: no unit needs any information from outside its own
synapse. Contrast backpropagation, which requires a globally propagated error
signal.

Geometrically, $\Delta w_{ij} = x_j y_i$ rotates the weight vector towards
inputs that already excite the unit — see
[[neural-similarity-and-dot-product]]. Hebbian learning builds templates for
whatever it sees most.

## Problems (L02, p7)

The notes flag these in orange — they are the reason [[stdp]] exists.

### 1. No synaptic weakening
**Hebb did not assume the existence of synaptic weakening.** With $x_j, y_i \ge 0$,
$\Delta w_{ij}$ is never negative, so weights only grow.

### 2. Self-amplification
Unbounded growth of weights. The feedback is positive with nothing opposing it:

```text
w grows -> y grows -> dw = x*y grows -> w grows faster -> ...   # divergence
```

Nothing in the rule bounds $w$. (The lecture does not offer a fix; the standard
remedies — weight normalisation, Oja's rule, weight decay — are `[external]` and
absent from the notes.)

### 3. Temporal symmetry
Since $\Delta w_{ij} = x_j y_i$, the rule is **temporally symmetric**: it
depends only on whether the two neurons are active, not on *which fired first*.
So it cannot distinguish "$j$ caused $i$" from "$i$ caused $j$" — it learns
correlation, not causation.

The notes sketch this with pairs of pre/post spike arrows in both orders,
producing the same outcome. This is exactly what [[stdp]] fixes.

## Where it appears in the module

- [[L02-spiking-neural-networks]] — page 7; named in the lecture's closing
  summary as *learning based on synaptic correlation*.

## See also

- [[stdp]] — Hebb's rule made a function of time; fixes problems 1 and 3.
- [[synaptic-plasticity]] — the general schema this instantiates.
- [[donald-hebb]]
- [[neural-similarity-and-dot-product]] — why the rule does something useful.
- [[perceptron-learning-rule]] (L03) — the supervised counterpart. The two rules
  differ by one substitution: `y` becomes `(t - y)`. That substitution is what
  makes the perceptron rule converge instead of self-amplifying.
- [[learning-paradigms]] — this is the *unsupervised* box.

## Additions from L04

L04 is where Hebb stops being described and starts being used. Two applications:

1. **[[multi-layer-associator]]** — word learning via action–perception
   correlation, with the rule written `Δw_ij = α·x_i·x_j` (coefficient named
   `α` rather than `η`).
2. **[[self-organising-map]]** — the lecture attributes *activation spreading*
   to Hebb's rule: activation is spread into the unit's direct neighbourhood, so
   *neighbours become sensitive to the same input patterns (Hebb's rule)*.

That second use is a stretch worth noting. The SOM update
`w ← w + h·η·(x − w)` is **not** Hebbian — it is an error-correcting move
*towards* the input, with a built-in decay term `−h·η·w`, which is exactly what
the bare Hebb rule lacks. The Hebbian flavour is in *who* learns (co-active
neighbours), not in *how* they learn. The notes do not make this distinction.

> [!note] Still no remedy for unbounded growth — as of L04
> Both L04 applications use the plain product form with no decay or
> normalisation — the self-amplification problem below remains unaddressed
> three lectures after it was introduced.
> **This is fixed at L06** — see below.

## Additions from L06 — a decay term at last

[[L06-hierarchical-vision]] is the **third** lecture to call something Hebbian,
and the first to write a rule that solves problem 2.

In [[dynamic-weight-sharing]], laterally connected units are updated during a
"sleep phase" by what the notes call the **Hebbian rule**:

```
Δw_i ∝ −( z_i − (1/N) Σ_{j=1}^{N} z_j ) · x  −  γ ( w_i − w_i^init )
                                                └──── decay ────┘
```

> [!success] Problem 2 is finally addressed
> `−γ(w_i − w_i^init)` is a **weight decay** term — the first in the module.
> Five lectures after self-amplification was flagged in orange on L02 p7, a rule
> appears with something opposing the positive feedback. It decays toward a
> reference `w_i^init` rather than toward zero, which additionally prevents the
> degenerate solution where all weights collapse and the network computes
> nothing.
>
> Note this is a *different* fix from the standard remedies listed above
> (normalisation, Oja's rule) — and closer to L03's [[regularisation]], which
> decays toward zero.

> [!warning] …but it is still not Hebb's rule
> `Δw ∝ −(z_i − z̄)·x` is **error-correcting**, not correlational. Worse, it is
> *anti*-Hebbian in sign: a unit firing **above** the population average has its
> weights **decreased**. Hebb's rule strengthens what fires together; this one
> suppresses whatever stands out.

## A finding: the module uses "Hebbian" to mean "local"

Three lectures, three rules called Hebbian, and only the first one is:

| Lecture | Rule | Form | Actually Hebbian? |
|---|---|---|---|
| L02 | Hebb's rule | `Δw = η·x_j·y_i` | **yes** — correlational |
| L04 | [[multi-layer-associator]] | `Δw = α·x_i·x_j` | **yes** — correlational |
| L04 | [[self-organising-map]] | `w ← w + h·η·(x − w)` | **no** — error-correcting, with decay |
| L06 | [[dynamic-weight-sharing]] | `Δw ∝ −(z_i − z̄)x − γ(w − w_init)` | **no** — error-correcting, anti-Hebbian in sign |

The pattern is consistent enough to state as a conclusion about the module's
vocabulary rather than as three separate errors:

> **The module uses "Hebbian" to mean *local* — an update computable from
> quantities available at the synapse — not *correlational*, which is what
> Hebb actually proposed.**

That usage is defensible as shorthand, since locality is the property that
matters for the biological-plausibility argument on
[[ann-brain-correspondence]]. But it is not what the L02 definition on this page
says, and the notes never acknowledge the widening.

## Open questions / gaps

- ~~No remedy for self-amplification is given.~~ **Resolved at L06** — see the
  decay term in [[dynamic-weight-sharing]]. Note it is never connected back to
  the L02 problem statement; the reader has to notice.
- $\eta$ is introduced with no guidance. Nor is `γ` in L06.
- The rule is applied to rate-coded units on this page but the module's real
  target is spiking units; the bridge is [[stdp]].
- **Still open:** no lecture has given a learning rule for a *spiking* network.
  Hebb and [[stdp]] are described in L02's spiking context, then every model
  that actually trains ([[self-organising-map]], [[multi-layer-associator]],
  [[dynamic-weight-sharing]]) uses rate-coded units.

## L13 — named as one of the two biological bases of continual learning

> **Bio: Hebbian learning & Complementary Systems Theory**

And [[gwr-network|GWR]] is described as **"Hebbian-like structural plasticity"**
— the edge rule, not the weight rule: units that win together get connected,
edges that are not co-activated age and die.

> [!note] Hebb applied to topology rather than to strength
> Classical Hebb adjusts a **weight** between two fixed neurons. GWR adjusts
> **whether the connection exists at all**, and whether the *neurons* exist.
> *Fire together, wire together* becomes *fire together, stay connected; fail to
> fire together, be deleted.*
>
> This is the module's most liberal reading of Hebb's rule, and also its most
> useful: it is what makes the network's structure track the data.

See [[complementary-learning-systems]] for the other half of the claim, which the
lecture names and never defines.
