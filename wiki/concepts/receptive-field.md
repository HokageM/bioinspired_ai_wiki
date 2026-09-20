---
type: concept
tags: [vision, neuroscience, convolution]
sources: [L06]
status: solid
---

# Receptive field

> **Region in which the presence of a stimulus will alter the firing of that
> neuron.**
> — [[L06-hierarchical-vision]], p3

The single most transferable idea in the lecture. It is simultaneously a fact
about neurons and the definition of a convolutional kernel's footprint.

## How it is established

From [[L06-hierarchical-vision]]:

- A light stimulus evokes an **action potential** in **ON-ganglion cells**.
- **Frequency increases with sensory strength** — this is [[rate-coding]].
- **Derivation of the AP determines the retinal area: the receptive field.**
  That is, the field is defined *empirically* — shine light in different places,
  see where firing changes, and that region is the field.

## Centre-surround organisation

| | Centre of RF | Periphery of RF |
|---|---|---|
| **ON cell** | **excitatory** | inhibitory |
| **OFF cell** | inhibitory | **excitatory** |

The notes give this as: *ON: excitatory influence on stimulus, centre of RF.
OFF: inhibitory influence on stimulus, periphery of RF.*

The p3 table tabulates spike trains for four stimuli against both cell types.
The behaviour it encodes:

| Stimulus | ON-centre cell | OFF-centre cell |
|---|---|---|
| diffuse full-field light | weak / no change | weak / no change |
| small spot on centre | **strong firing** | suppressed |
| light on surround only | suppressed | **strong firing** |
| large disc covering both | intermediate | intermediate |

**The key consequence: uniform light produces no response.** Excitation and
inhibition cancel. A neuron with a centre-surround field is a *contrast*
detector, not a *brightness* detector.

This is why [[the-retina]]'s output is already an edge map before anything
reaches the cortex — and why [[convolutional-network]]'s first-layer kernels,
when trained on natural images, come out looking like centre-surround and
oriented-edge filters.

## Pseudocode

```
# Empirical procedure — how a receptive field is actually measured
function map_receptive_field(neuron, retina_positions):
    baseline = firing_rate(neuron, no_stimulus)
    field = {}
    for p in retina_positions:
        r = firing_rate(neuron, spot_of_light_at(p))
        if   r > baseline: field[p] = "ON"    # excitatory
        elif r < baseline: field[p] = "OFF"   # inhibitory
    return field                              # the shape of `field` is the RF
```

```
# Difference-of-regions model of the response
response_ON(x)  =  w_c · Σ(light in centre)  −  w_s · Σ(light in surround)
# w_c, w_s > 0 and chosen so that uniform light ⇒ response ≈ 0
```

## The same idea, three times

In [[L06-hierarchical-vision]] the phrase "receptive field" is used for:

1. the retinal area that modulates a ganglion cell's firing (biology);
2. the region a [[neocognitron]] S-cell reads from;
3. the parameter **`r`** in the convolution formula
   `z_i = Σ_{j=0}^{r−1} w_j x_{i+j}` and the pooling parameter `r_p`.

The lecture never explicitly says these are the same concept — it simply uses
the word for all three, which is arguably the clearest evidence in the whole
module that the CNN was *derived from* the cortex rather than merely compared to
it. Contrast [[ann-brain-correspondence]], where most claimed correspondences
lack any such shared vocabulary.

## Related

[[the-retina]] · [[simple-complex-hypercomplex-cells]] · [[orientation-tuning]]
· [[convolutional-network]] · [[neocognitron]] · [[pooling]] ·
[[excitatory-and-inhibitory-neurons]] · [[rate-coding]] ·
[[local-vs-distributed-representation]] · [[L06-hierarchical-vision]]
