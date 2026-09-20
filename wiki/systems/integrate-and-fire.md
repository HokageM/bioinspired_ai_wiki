---
title: Integrate and Fire Model
type: system
tags: [spiking, dynamics, neuroscience]
sources: [L02]
created: 2026-09-20
updated: 2026-09-20
status: solid
---

# Integrate and Fire Model

The first of the module's two spiking models (L02, p6): a
[[continuous-dynamic-neuron]] with a threshold, a reset, and strong negative
feedback after each spike.

## Biological origin

It reproduces the two defining features of an [[action-potential]]: the membrane
charges up until it crosses a threshold, then fires a stereotyped all-or-none
spike and resets. The negative feedback reproduces the [[refractory-period]] —
the stated purpose is **to prevent continuous spike emission** (L02, p6).

## Computational form

The margin formulae, as written in the notes:

$$\tau\frac{d}{dt}u_i = -a_i + R\,I(t) \quad \text{// integration}$$
$$u_i(t) = \theta \quad \text{// fire \& reset, threshold}$$

Structure (L02, p6):

```
 x1 --w_i1--\
 x2 --w_i2---> [ Σ ] --> [ τ_i ] --a_i(t)--> [ ⌐| threshold ] --> [ Π spike ] --> y_i
 xN --w_iN--/     ↑                                   ↑                     |
                  |______________ -r_i ______________ | ____________________|
                        strong negative feedback after spike
```

```text
# integrate and fire
u = u_rest

for t in 0 .. T step dt:

    # --- INTEGRATE ---
    I  = sum over j of w[i][j] * x[j](t)
    du = (-u + R * I) / tau
    u  = u + dt * du

    # --- FIRE & RESET ---
    if u >= theta:
        emit_spike(at = t)          # Dirac impulse delta(t); all-or-none
        u = u - r                   # strong negative feedback -r_i
                                    # prevents continuous spike emission
                                    # = [[refractory-period]]
    else:
        pass                        # sub-threshold: nothing is emitted
```

The entire model is those two phases. Between spikes it is a leaky integrator —
identical to [[continuous-dynamic-neuron]]; at threshold it becomes discrete.
That hybrid is what makes it a spiking model: continuous dynamics, discrete
output.

Note the consequence of *strong* (fixed, large) negative feedback: it imposes a
hard ceiling on the firing rate, since $u$ must climb all the way back from
$u_{rest} - r$. The [[spike-response-model]] softens exactly this.

## Where it appears in the module

- [[L02-spiking-neural-networks]] — page 6; named in the lecture's closing
  summary as one of the three models to know.

## See also

- [[spike-response-model]] — the alternative; differs **only** in the inhibitory
  feedback (L02).
- [[continuous-dynamic-neuron]] — the sub-threshold dynamics.
- [[refractory-period]], [[action-potential]]
- [[spiking-neural-network]] — the network this unit composes into.

## Open questions / gaps

> [!note] Notation drift in the source
> The differential equation mixes symbols: $\tau\frac{d}{dt}u_i = -a_i + RI(t)$
> uses $u_i$ on the left and $a_i$ on the right. They are clearly the same
> membrane variable; the notes switch between the "activation" and "membrane
> voltage" naming without comment.

- $R$ appears without definition — from page 5's RC reading it is the membrane
  resistance, but the notes do not say so here.
- No value or functional form for $r_i$, and no reset value is specified.
- The notes write "fire & reset" and "$u_i(t) = \theta$" together, which reads
  as a threshold *condition* rather than a reset *assignment*; the intended
  reset target is not given.
- Leaky vs non-leaky integrate-and-fire is not distinguished.
