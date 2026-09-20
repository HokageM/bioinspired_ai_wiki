---
title: Hodgkin-Huxley Model
type: system
tags: [neuroscience, dynamics, biophysics]
sources: [L02]
created: 2026-09-20
updated: 2026-09-20
status: stub
---

# Hodgkin-Huxley Model

> [!note] Stub — named without definition
> The Hodgkin and Huxley model is mentioned exactly once in the notes, as the
> thing that [[continuous-dynamic-neuron]] simplifies. No equations, no
> mechanism, no dates and no biography are given (L02, p5).

## What the source actually says

From L02, page 5, in full:

> "Simplification of so-called **Hodgkin and Huxley Model**
> → Neuron Model: **voltage difference of a capacitor**
> → $\tau$ is approximated by a resistor"

So the module's position is: the Hodgkin-Huxley model is the detailed
biophysical account, and

$$\frac{da_i(t)}{dt} = -\frac{1}{\tau}a_i(t) + \sum_j w_{ij}x_j(t)$$

is the tractable RC-circuit approximation of it. That approximation is what the
module actually uses.

## Computational form

None given. The RC simplification that replaces it:

```text
# what the module uses INSTEAD of Hodgkin-Huxley
da/dt = -(1/tau) * a + drive        # one state variable, one time constant

# [external] the Hodgkin-Huxley model proper has four coupled state variables
# (membrane voltage plus three gating variables) and separate conductances for
# sodium, potassium and leak currents. None of this appears in the notes.
```

## Where it appears in the module

- [[L02-spiking-neural-networks]] — page 5, one line.

## See also

- [[continuous-dynamic-neuron]] — the simplification the module works with.
- [[action-potential]] — the phenomenon Hodgkin-Huxley explains mechanistically.
- [[integrate-and-fire]], [[spike-response-model]] — further simplifications.

## Open questions / gaps

- Everything. This page exists so links resolve and so the gap is visible.
- **Worth chasing:** a squid doodle appears in the page-1 margin of the notes
  next to the dendrite/axon sketch. Hodgkin and Huxley worked on the squid giant
  axon, so this is plausibly a mnemonic connecting the two pages — but the notes
  never state it. See [[L02-spiking-neural-networks]] § Unclear in the source.
- A candidate lint action: fill this page from an external source, clearly
  marked `[external]`, or confirm from the lecture slides whether the module
  covers it in more depth elsewhere.
