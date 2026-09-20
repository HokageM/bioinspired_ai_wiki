---
title: Discrete Dynamic Neuron Model
type: system
tags: [dynamics, recurrent, neural-networks]
sources: [L02]
created: 2026-09-20
updated: 2026-09-20
status: solid
---

# Discrete Dynamic Neuron Model

A [[mcculloch-pitts-neuron]] given a **memory of its own past activation**, via a
recurrent self-connection and a time delay (L02, p4).

## Biological origin

Indirect — this is a repair to a modelling limitation rather than a copy of a
mechanism. The motivation given (L02, p4):

- **Before:** output is determined by the current input; no temporal processing.
- **Now:** temporal dependencies (e.g. speech / language).
- **Realise in static networks:** a **feedback loop between the output from time
  $t_0$ and the input at time $t_1$ provides a temporal delay of the signal into
  the future.**

## Computational form

Two components are added to the static unit (L02, p4):

- $\Delta$ — the **time delay** between the current activation $a_i(n)$ and the
  update step $a_i(n+1)$. The delay *keeps* the activation $a_i$ for an amount
  of $\Delta$.
- $\mu_i$ — the **recurrent connection weight**.

**Update:**
$$a_i(n+1) = \mu_i\,a_i(n) + \sum_{j=1}^{N} w_{ij}x_j(n)$$

**Output:**
$$y_i(n) = \phi\big(a_i(n) - \theta_i\big)$$

Structure:

```
 x1 --w_i1--\
    ...      > [ Σ ] --a_i(n+1)--> [ Δ ] --a_i(n)--> [ φ() ] --> y_i(n)
 xN --w_iN--/     ↑                          |            ↑
                  |___________ μ_i __________|           θ_i
```

```text
# discrete dynamic neuron — simulate over n steps
a = 0                                   # activation state PERSISTS across steps
for n in 0 .. T:
    drive = sum over j of w[i][j] * x[j][n]       # current input
    a     = mu[i] * a + drive                     # leaky accumulation
    y[n]  = phi(a - theta[i])                     # output

# mu controls memory:
#   mu = 0    -> a = drive          : no memory, reduces to the STATIC unit
#   0 < mu < 1-> geometric decay    : exponentially fading memory
#   mu = 1    -> perfect integrator : never forgets
#   mu > 1    -> divergence
```

The whole model is the single term $\mu_i a_i(n)$. With it, the unit's output
depends on the entire input history, weighted by $\mu_i^{k}$ for an input $k$
steps ago — an exponentially decaying trace. The notes sketch **activation decay
for different $\mu$**, with $\mu = 0.7$ as the worked case.

Note the sequencing: $\phi$ is applied to $a_i(n)$, the activation *before* the
current update — the delay $\Delta$ sits between the summation and the output.

## Where it appears in the module

- [[L02-spiking-neural-networks]] — page 4.

## See also

- [[continuous-dynamic-neuron]] — the same model in continuous time; there
  $\mu$ corresponds to $1/\tau$.
- [[mcculloch-pitts-neuron]] — the $\mu = 0$ special case.
- [[network-architectures]] — this is the recurrent architecture reduced to one
  unit.
- [[integrate-and-fire]] — what this becomes once a fire-and-reset rule is added.

## Open questions / gaps

- The $\mu = 0.7$ decay plot on page 4 is sketched without values, so the curve
  cannot be reconstructed exactly.
- **No learning rule for $\mu_i$.** It is treated as a fixed parameter.
- Stability is not discussed — the notes never state that $\mu_i < 1$ is
  required to avoid divergence.
- The relationship between $\Delta$ (a delay) and $\mu_i$ (a weight) is not
  formalised; they are drawn as separate blocks but only $\mu_i$ appears in the
  equation.
