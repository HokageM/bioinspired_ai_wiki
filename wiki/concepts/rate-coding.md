---
title: Rate Coding
type: concept
tags: [coding, neural-networks]
sources: [L02]
created: 2026-09-20
updated: 2026-09-20
status: solid
---

# Rate Coding

Representing a neuron's activity as a **single number — its activity level** —
rather than as a sequence of discrete spikes.

## Biological origin

The justification given in the lecture's own summary: *the activation state of a
neuron is approximated by its firing rate* (L02, final page). Since an
[[action-potential]] is all-or-none, the number of spikes per unit time is a
real quantity the brain could plausibly use, and averaging it away yields a
continuous value.

## Computational form

Rate coding is what makes the [[mcculloch-pitts-neuron]] possible: with one
number per neuron, a network's state is a vector and its connectivity is a
**weight matrix** (L02, p2).

$$y_i = \phi\left(\sum_j w_{ij}x_j - \theta_i\right)$$

```text
# rate-coded forward pass — the whole left branch of [[neural-coding]]
input:  x[1..N]        # activity levels, real-valued
        W[i][j]        # weight matrix
        theta[i]       # thresholds (or fold into a bias unit)
        phi            # [[activation-function]]

for each unit i:
    A[i] = sum over j of W[i][j] * x[j] - theta[i]
    y[i] = phi(A[i])

return y
```

Note what is absent: **there is no time variable.** The output is fully
determined by the current input — the lecture calls this "rather static" (L02,
p4) and spends the rest of the lecture escaping it.

## The cost

| Kept | Lost |
|---|---|
| Magnitude of activity | Spike times |
| Cheap dense linear algebra | Synchrony between neurons |
| Differentiability (via $\phi$) | Relative delays |

Everything in the "Lost" column is recovered by [[temporal-coding]].

## Where it appears in the module

- [[L02-spiking-neural-networks]] — page 2 (the fork), page 2–3 (the unit),
  page 4 (its static limitation).

## See also

- [[neural-coding]] — the fork this is one branch of.
- [[mcculloch-pitts-neuron]] — the unit built on it.
- [[temporal-coding]] — the alternative.
- [[discrete-dynamic-neuron]] — the first repair to the static limitation.

## Open questions / gaps

- The notes do not say over what time window a "firing rate" is averaged, which
  is the practical crux of whether rate coding is biologically defensible.
- "Rate-coded" appears in quotation marks in the notes, suggesting the lecturer
  flagged it as a term of art, but no further comment is recorded.
