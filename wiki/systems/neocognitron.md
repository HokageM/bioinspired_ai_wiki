---
title: Neocognitron
type: system
tags: [neural-networks, vision, hierarchy]
sources: [L06]
created: 2026-09-21
updated: 2026-09-21
status: solid
---

# Neocognitron

**[[kunihiko-fukushima]]'s** hierarchical multilayer neural network for visual
recognition — the missing link between [[david-hubel]] and [[torsten-wiesel]]'s
cortical recordings and [[yann-lecun]]'s [[convolutional-network]].

## As given in L06

From [[L06-hierarchical-vision]], p6:

- Hierarchical multilayer NN for visual recognition.
- **Alternate planes of simple S-cells (feature extraction) and complex C-cells
  (positional errors).**
- **Resemble processing stages in the visual cortex.**
- **S-cells trained to a particular feature present in a receptive field.**
- **C-cells inserted to correct for positional errors: receive responses from
  S-cells coding for the same feature.**
- **Training of S-cells with unsupervised or supervised methods; only S-cells
  have learning inputs.**

## Architecture

```
In → U_C0 → U_S1 → U_C1 → U_S2 → U_C2 → … → Recognition Layer
      ↑        ↑       ↑
   contrast  simple  complex
  extraction  cells   cells
```

`U_C0` performs **contrast extraction** — the model's stand-in for retinal
centre-surround processing (see [[the-retina]]), applied before any feature
detection begins.

Thereafter the network alternates strictly: **detect, then discard position;
detect, then discard position.** Each S-plane finds one feature type across the
image; each C-plane makes that detection robust to small shifts.

## The correspondence

| Cortex ([[simple-complex-hypercomplex-cells]]) | Neocognitron | CNN |
|---|---|---|
| retinal centre-surround | `U_C0` contrast extraction | (implicit in first conv) |
| simple cell | **S-cell** | convolution layer |
| complex cell | **C-cell** | **[[pooling]]** layer |
| V1 → V2 → … | S/C plane stack | conv/pool blocks |

The vocabulary makes the derivation explicit: Fukushima named his units after
the cells Hubel and Wiesel recorded. This is why the CNN↔cortex correspondence
is one of the few in the module that holds up — see
[[ann-brain-correspondence]].

## Only S-cells learn

The most architecturally interesting detail, and one the notes state plainly:
**only S-cells have learning inputs.**

C-cells are **fixed**. They are not trained; they are *wired* to pool over
S-cells coding for the same feature. Invariance is therefore built into the
architecture rather than discovered from data — the same design decision made
by [[pooling]] in a modern CNN, which also has no parameters.

So the division of labour is: **learned detection, hard-wired invariance.**

## Training paradigm

**S-cells may be trained with unsupervised *or* supervised methods.**

This is notable in the context of [[learning-paradigms]]. The same layer,
computing the same function, can be fitted either way — which supports the
module's recurring claim that the paradigm is a property of the *training
signal*, not of the architecture. Compare [[self-supervised-learning]] in L04,
where [[gpt]] and [[hubert]] have near-identical setups labelled differently.

## Pseudocode

```
function neocognitron_forward(image):
    u = contrast_extract(image)            # U_C0
    for level in 1 .. L:
        u = s_plane(u, level)              # learned feature detection
        u = c_plane(u, level)              # fixed positional tolerance
    return recognition_layer(u)
```

```
function s_plane(u, level):
    # one plane per feature type; each S-cell looks at a local receptive field
    out = {}
    for k in feature_types[level]:
        for position in positions:
            out[k][position] = rectify(
                sum( w[level][k][j] * u[position + j] for j in receptive_field )
            )
    return out                             # w is LEARNED

function c_plane(u, level):
    # pool S-cells of the SAME feature k over nearby positions
    out = {}
    for k in feature_types[level]:
        for position in positions:
            out[k][position] = pool(
                u[k][position + d] for d in tolerance_window
            )
    return out                             # NO learnable parameters
```

```
# Unsupervised S-cell training, in the spirit of the source's
# "trained to a particular feature present in a receptive field":
function train_s_cell_unsupervised(patches):
    for patch in patches:
        winner = argmax over k of response(w[k], patch)   # competitive
        w[winner] += eta * (patch - w[winner])            # move toward input
```

> [!note] The last block is reconstructed, not quoted
> The notes say only that S-cells can be trained unsupervised; they give no
> rule. The competitive/winner-take-all form above is `[external]`, chosen
> because it matches the [[self-organising-map]] update the module already used
> in L04 and produces the described outcome — each S-cell becoming tuned to one
> feature.

## Unclear in the source

- **No equations.** The Neocognitron is described entirely in prose and a block
  diagram. S-cell and C-cell responses are never written down.
- **No learning rule** for the S-cells, despite training being discussed.
- **"Correct for positional errors"** is the only description of what C-cells
  do. That it amounts to pooling is inferable from the parallel with complex
  cells and with the CNN's subsampling layers, but is not said.
- **No date, no performance, no dataset.** `[external]` The Neocognitron dates
  from 1980, predating [[backpropagation]]'s popularisation — which is precisely
  why it needed unsupervised S-cell training and hard-wired C-cells. The notes
  omit this context, which would have explained the design.
- **How many levels `L`?** Not stated.

## See also

[[convolutional-network]] · [[kunihiko-fukushima]] ·
[[simple-complex-hypercomplex-cells]] · [[pooling]] · [[receptive-field]] ·
[[the-retina]] · [[learning-paradigms]] · [[self-organising-map]] ·
[[ann-brain-correspondence]] · [[levels-of-abstraction]] ·
[[L06-hierarchical-vision]]
