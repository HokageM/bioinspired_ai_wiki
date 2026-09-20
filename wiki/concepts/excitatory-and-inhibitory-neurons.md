---
title: Excitatory and Inhibitory Neurons
type: concept
tags: [neuroscience]
sources: [L02, L06, L08]
created: 2026-09-20
updated: 2026-09-21
status: developing
---

# Excitatory and Inhibitory Neurons

The two types of neuron distinguished in the module (L02, p2):

1. **Excitatory** — increases the activation of its targets.
2. **Inhibitory** — decreases it.

## Biological origin

Stated as a plain taxonomy with no further elaboration in the notes.

## Computational form

The distinction is carried in the **sign of the weight**, which is exactly how
the lecture's weight-matrix visualisation encodes it (L02, p2): a matrix of
neuron-ID (from) against neuron-ID (to), where

- **black → $w < 0$** (inhibitory)
- **white → $w > 0$** (excitatory)

```text
# type is a property of the *source* neuron, not of the individual synapse
for each neuron j:
    if type(j) == EXCITATORY:
        w[i][j] >= 0  for all targets i
    else:                                  # INHIBITORY
        w[i][j] <= 0  for all targets i

# a plain [[mcculloch-pitts-neuron]] does NOT enforce this:
# there, any single neuron may have both positive and negative outgoing weights.
```

That constraint — sometimes called Dale's principle — is `[external]`; the notes
only give the black/white sign convention and the two-type list, not the rule
that a neuron must be consistently one or the other.

## Where it appears in the module

- [[L02-spiking-neural-networks]] — page 2, immediately before the modelling
  section.

## See also

- [[mcculloch-pitts-neuron]] — where signed weights first appear.
- [[refractory-period]] — inhibition used as self-feedback rather than between
  neurons.
- [[action-potential]]

## Open questions / gaps

- The notes give no proportion, no neurotransmitters, and no example circuits.
- Inhibition reappears on page 6 as *self*-inhibition (refractoriness) without
  the lecture connecting it back to inhibitory neurons. Whether the module
  intends these as the same mechanism is unstated.


## L06: excitation and inhibition arranged in space

[[L06-hierarchical-vision]] gives the module's clearest functional use of the
E/I distinction. In the retinal [[receptive-field]], excitation and inhibition
are not properties of *different cells* but of *different regions of the same
cell's input*:

| | Centre of RF | Periphery of RF |
|---|---|---|
| **ON cell** | **excitatory** | inhibitory |
| **OFF cell** | inhibitory | **excitatory** |

The point of the arrangement is cancellation. Because the two regions oppose
each other, **uniform illumination produces no response** ? the cell is blind to
absolute brightness and reports only contrast. Inhibition here is not a brake on
activity; it is what makes the computation a *derivative*.

The same structure appears in the model as a zero-sum convolution kernel, e.g.
L06's horizontal line filter `(-1 -1 -1 / 2 2 2 / -1 -1 -1)`, whose negative
weights are the inhibitory surround written out.

Lateral inhibition of this kind is also the mechanism behind the
[[self-organising-map]]'s competitive dynamics in L04, though neither lecture
connects them.

See [[the-retina]] and [[convolutional-network]].


## Inhibition as an architectural primitive (L08)

[[L08-behaviour-based-robotics]] uses the same two signs at the level of whole
behaviours rather than synapses:

- A [[braitenberg-vehicle]]'s connections may be excitatory (*more light,
  faster*) or **inhibitory** ? *stronger stimulus ? smaller input to actuator*.
  Flipping the sign turns *fear* into *admires* and *aggression* into
  *explores*: the wiring is unchanged, only the sign.
- [[subsumption-architecture]] gives higher layers the power to **inhibit or
  subsume** lower ones.

So the module now has inhibition doing four jobs: balancing excitation in a
neuron (L02), sharpening competition in a [[self-organising-map|SOM]] (L04),
suppressing a whole behaviour (L08) ? and, in L07, the strictly weaker
[[top-down-modulation|modulatory]] case, which biases rather than blocks.

That last distinction is worth keeping: *suppress* replaces a signal, *modulate*
reweights it, and only the second leaves the lower stage able to contribute.
