---
title: Continuous Dynamic Neuron Model
type: system
tags: [dynamics, neural-networks, neuroscience]
sources: [L02]
created: 2026-09-20
updated: 2026-09-20
status: solid
---

# Continuous Dynamic Neuron Model

The [[discrete-dynamic-neuron]] taken to continuous time: activation is governed
by a differential equation rather than a step update (L02, p5).

## Biological origin

Explicitly stated (L02, p5):

- It **displays the exponential dynamics of activation at the neuron's
  membrane**.
- It is a **simplification of the so-called Hodgkin and Huxley model** — see
  [[hodgkin-huxley-model]].
- The neuron is read as an electrical circuit: **voltage difference of a
  capacitor**, with **$\tau$ approximated by a resistor**.

That last reading is the useful one — the neuron is an RC circuit. The membrane
holds charge like a capacitor; current leaks away through a resistance; $\tau$ is
the product that sets how fast.

## Computational form

**Derivative of activation over time $t$:**

$$\frac{da_i(t)}{dt} = -\frac{1}{\tau}a_i(t) + \sum_{j=1}^{N} w_{ij}x_j(t)$$

From the notes (L02, p5):

- $\tau$ is a **time constant whose magnitude is inversely proportional to the
  decay rate** of the activation, for $\tau > 0$.
- The **update is a derivative of activation over time.**
- $\mu \approx$ **exponential decay** $e^{-t/\tau_i}$.

### The correspondence between the two models

$$\mu \;\; (\text{discrete case}) \quad \longleftrightarrow \quad
\frac{1}{\tau} \;\; (\text{continuous case})$$

This is the identity to remember. Large $\mu$ = long memory; large $\tau$ = slow
decay = long memory; hence $\mu \leftrightarrow 1/\tau$ as the notes write it.

```text
# continuous dynamic neuron, integrated numerically (forward Euler)
a = 0.0
for t in 0 .. T step dt:
    drive = sum over j of w[i][j] * x[j](t)
    da    = -(1.0 / tau) * a + drive         # the ODE
    a     = a + dt * da                      # integrate
    y(t)  = phi(a - theta)                   # [external] L02 gives phi for the
                                             # discrete model; applying it here
                                             # follows the compact diagram

# with no input (drive = 0) the solution is exactly a(t) = a(0) * exp(-t / tau):
# the "exponential dynamics of activation at the neuron's membrane".

# equivalence with [[discrete-dynamic-neuron]]:
#   mu  ==  1 - dt / tau   ~  exp(-dt / tau)
# so the discrete model is this model sampled at interval dt.
```

The notes also give a **compact representation** of the unit — the summation
feeds a block labelled with a decaying-exponential icon and $\tau_i$, producing
$a_i(t)$, which feeds $\phi()$:

```
 [ Σ ] --> [ ⌐\__  τ_i ] --a_i(t)--> [ φ() ]
```

## Where it appears in the module

- [[L02-spiking-neural-networks]] — page 5, between the discrete model and the
  spiking models.

## See also

- [[discrete-dynamic-neuron]] — the sampled version; note the $\mu = 1/\tau$
  correspondence.
- [[hodgkin-huxley-model]] — what this simplifies.
- [[integrate-and-fire]] — this ODE plus a fire-and-reset rule.
- [[synaptic-kernel]] — the immediate next step, giving each *input* its own
  temporal characteristic rather than sharing one $\tau$.

## Open questions / gaps

- The output equation is not restated for the continuous case; $\phi$ is carried
  over from the discrete model on the basis of the compact diagram.
- The RC analogy names the capacitor and resistor but no capacitance value or
  full circuit equation is given.
- **Nothing about numerical integration** — no mention of step size or
  stability, although the whole model is an ODE.
- What "$\tau$ approximated by a resistor" means precisely is left implicit
  ($\tau = RC$ is `[external]`).
