---
title: Donald Hebb
type: entity
tags: [people, learning, plasticity]
sources: [L02]
created: 2026-09-20
updated: 2026-09-20
status: stub
---

# Donald Hebb

The originator of [[hebbian-learning]], the module's first and simplest learning
rule.

> [!note] Stub — named without biography
> The notes name Hebb only through "Hebb's rule" and one substantive claim about
> what he assumed. No dates, nationality, publication or biographical detail
> appears anywhere in L01–L02. The first name "Donald" is `[external]`; the
> notes write only "Hebb".

## What the source says

- **Hebb's rule:** *"Cells that fire together, wire together"* — activation
  correlations strengthen synaptic connections (L02, p7).
- The rule is described as **the simplest and most general form of learning**
  (L02, p7).
- **Hebb did not assume the existence of synaptic weakening** (L02, p7).

That last point is the only claim in the notes about Hebb as a person rather
than about the rule, and it is the substantive one. It is the root cause of both
failure modes recorded on [[hebbian-learning]]:

- without weakening, $\Delta w_{ij} = x_j y_i \ge 0$ always, so weights suffer
  **self-amplification** (unbounded growth);
- and the rule is **temporally symmetric**, unable to distinguish which neuron
  fired first.

[[stdp]] is precisely the repair: it reintroduces weakening by making the update
a function of spike *timing*, so weights can decrease.

## Where it appears in the module

- [[L02-spiking-neural-networks]] — page 7.

## See also

- [[hebbian-learning]] — the rule.
- [[synaptic-plasticity]] — the phenomenon it describes.
- [[stdp]] — the successor that supplies what Hebb omitted.

## Open questions / gaps

- No dates, no citation, no institution.
- The notes do not say *why* Hebb did not assume weakening, or when synaptic
  weakening (long-term depression) was established.
- No other named researchers appear in L01–L02 except McCulloch, Pitts, Hodgkin
  and Huxley — all of whom are likewise mentioned only via model names.
