---
type: lecture
lecture: L06
title: Bio-inspired Hierarchical Vision
source: raw/lectures/BioinspiredAIWdh6.pdf
pages: 9
dates_on_pages: [2024-02-06]
tags: [vision, hierarchy, convolution, neuroscience, plausibility]
---

# L06 — Bio-inspired Hierarchical Vision

> Source: `raw/lectures/BioinspiredAIWdh6.pdf`, 9 pages, all marked **L6**,
> dated 06.02.2024.

## Summary

The lecture the module has been building towards. Its argument is a single
sustained analogy between the visual cortex and the convolutional network, and
unlike most of the module's correspondences, this one is historically real: the
CNN was *actually derived* from the cortex, via [[kunihiko-fukushima]]'s
[[neocognitron]].

The structure is a ladder, climbed twice:

| Biology | Model |
|---|---|
| [[receptive-field]] (ON/OFF centre-surround) | convolutional kernel |
| simple cells — edge detectors | first conv layer |
| complex cells — translation-invariant | pooling |
| hypercomplex cells — angles, lengths | deeper conv layers |
| V1 → V2 → V3 → V5 | stacked layers |

And then, in its last third, the lecture turns critical. **CNNs require weight
sharing, which real neurons cannot do.** So the analogy breaks precisely where
it matters most, and the lecture spends two pages proposing repairs —
[[data-augmentation]] and [[dynamic-weight-sharing]].

That final section is the most important thing in the module so far for the
wiki's standing question about biological plausibility. See
[[ann-brain-correspondence]].

> [!important] L06 confirms an error flagged in L04
> On p4 this lecture states **dorsal = *where*, ventral = *what***. L04 labelled
> comprehension (*sound to meaning*) as **dorsal**. Meaning is *what*, and *what*
> is **ventral**. The two lectures contradict each other and L06 is correct.
> The flag raised on [[dual-stream-hypothesis]] is now confirmed **by the
> module's own later material** rather than by external reference.

## Key ideas

### Fields of view

**Different visual fields of view in nature.** e.g. rabbit vs human ⇒ the human
brain is more complex ⇒ it **integrates information from both eyes**. The rabbit
sketch shows laterally-placed eyes with little overlap; the human, forward-facing
eyes with a large binocular field.

### The [[visual-pathway]]

```
foveal image → left/right eye → optic nerve → optic tract
   → LGN (Lateral Geniculate Nucleus)  →  striate cortex
   → superior colliculus
```

**Stimuli from the retina project to four subcortical regions:**

| Region | Function |
|---|---|
| **Lateral Geniculate Nucleus (LGN)** | provides input to **V1** (edge detection) |
| **Hypothalamus** | controls the **circadian cycle** (body clock) |
| **Pretectum** | controls the **pupillary light reflex** |
| **Superior Colliculus** | controls **eye movements** |

### Human visual areas

Visual information processing in the **occipital lobe** (back of brain).
Different areas for different visual information:

- **V1:** edge detection
- **V2:** more complex patterns, parts of figures
- **V3:** colour, motion
- **(V4:** orientation)
- **V5:** motion, eye movements

### [[the-retina]]

```
Light ↓
Optic Nerve Fibers → Ganglion Cells → Inner Synaptic Layer
  → Cells (Amacrine, Bipolar, Horizontal) → Outer Synaptic Layer
  → Receptor Nuclei → Receptors Pigmented Layer
```

**Inside: the rod and the cone cells.**

- **Cone** — colour vision; at the fovea; functions best in bright light.
- **Rod** — almost entirely responsible for night vision; outer edge of retina.

Both have a synaptic ending, inner segment and outer segment containing
**photopigment**.

### [[receptive-field]]

> **Region in which the presence of a stimulus will alter the firing of that
> neuron.**

- Light stimulus evokes an **action potential** in **ON-ganglion cells**.
- **Frequency increases with sensory strength.**
- ~~Derivation~~ **Derivation of AP determines retinal area: receptive field.**
- **ON:** excitatory influence on stimulus, **centre** of RF.
- **OFF:** inhibitory influence on stimulus, **periphery** of RF.

The four-row response table on p3 shows how firing changes for a diffuse
surround, a small central spot, a large spot and an offset spot.

### [[simple-complex-hypercomplex-cells]] — V1 cell types

- **Simple cells:** line/edge detector, **highly selective**
- **Complex cells:** orientation detector, **implements invariant features**,
  e.g. translational invariance
- **Hypercomplex cells:** angle/length detector; **composed of several complex
  cells**

### [[david-hubel]] and [[torsten-wiesel]]

- Cells in **striate cortex** respond best to **bars of light** rather than to
  spots of light.
  - simple cells: bars of light **/** bars of dark
  - complex cells: bars of light **&** bars of dark
- **[[orientation-tuning]]:** tendency of neurons in striate cortex to respond
  more to bars of certain orientations and less to others. **Response rate falls
  off with the angular difference of the bar from the preferred orientation.**
  The tuning curve on p4 peaks at 0° and decays to zero by ±90°.

**Simple cell processing:** a) edge detector, b) stripe detector — drawn as
`+`/`−` subfield arrangements.

### [[two-visual-streams]]

**Higher visual processing** — *dichotomy of object processing* in the occipital
lobe. **Partitioning found by [[leslie-ungerleider]]**; separation underpinned by
**lesion studies**.

| Stream | Projects to | Encodes | Question |
|---|---|---|---|
| **Ventral** | temporal cortex | shape, colour, texture | **what?** |
| **Dorsal** | parietal cortex | object spatial information | **where?** |

The dorsal stream is *also sometimes referred to as the **how**-path, due to
sensory-motor transformation.*

`eg:` **NN for ventral & dorsal stream.** *How about storing multiple sensory
sequences in one RNN? Can the sensory info still be encoded in two streams?*
The sketch shows a shared module `μ` feeding separate **ventral layers** and
**dorsal layers**, which recombine.

### [[neocognitron]] ([[kunihiko-fukushima]])

Hierarchical multilayer NN for visual recognition.

- **Alternate planes of simple S-cells (feature extraction) and complex C-cells
  (positional errors).**
- **Resemble processing stages in the visual cortex.**
- `In → U_C0 → U_S1 → U_C1 → … → Recognition Layer` (with contrast extraction
  at the front).
- **S-cells trained to a particular feature present in a receptive field.**
- **C-cells inserted to correct for positional errors: receive responses from
  S-cells coding for the same feature.**
- **Training of S-cells with unsupervised or supervised methods; only S-cells
  have learning inputs.**

### [[convolutional-network]] ([[yann-lecun]])

The LeNet-5 diagram on p6:

```
32×32 in → 6@28×28 → 6@14×14 → 16@10×10 → 16@5×5 → 120 → 84 → 10
         conv      subsampling   conv    subsampling  full   Gaussian
                                                    connection connections
```

**Convolutional kernels as filters.** `eg:` a horizontal line filter

```
−1 −1 −1
 2  2  2
−1 −1 −1
```

applied to an image ⇒ **convolved feature / activation map**.

### Formalisation of a convolutional layer (1D)

```
z_i = Σ_{j=0}^{r−1}  w_j · x_{i+j},     i ∈ [0, N]
```

- **Receptive field size** `r = 3`
- **Stride** `s` — *how much you jump*; `s = 1`
- **Number of patches** `N = (|x| − r)/s + 1`
- **Zero padding `p`** — amount of elements added to the input vector to obtain
  an output vector of the same size as the input (i.e. `|x| = |z|`)

With multiple filters — **each filter creates an activation map**:

```
z_{i,k} = Σ_{j=0}^{r−1}  w_{j,k} · x_{i+j}
```

*Activation of the i-th neuron on the k-th activation map.*

2D with multiple filters: **dimension of output `n_f × r` for `n_f` filters**;
**dimensionality reduction by [[pooling]]**.

### [[pooling]]

`max` (or `average`) over a window. Pooling layers with receptive field
`r_p = 2` and stride `s_p = 2`: `dim(z) = 8×2` ⇒ `dim(p) = 4×2`.

### Biologically plausible CNNs

> **CNNs require weight sharing, which real neurons cannot do.**
> (Margin: *to detect common local features across an image by applying the same
> weight.*)

> **Locally connected networks do not share weights but perform worse than CNNs
> on image classification tasks.**

```
  locally connected            convolutional
     w_1 ≠ w_4                    w_1 = w_4
```

**How to bridge the gap between the biologically plausible locally connected
network and the well-performing but less plausible CNN?**

**Two mechanisms:**

1. **[[data-augmentation]] via multiple image translations**
   - Done by showing a locally connected network multiple translations of the
     same image **simultaneously**.
   - This makes neurons in a channel react similarly to the same input.
   - ⚠ **Requires longer training times**; ⚠ **only small performance
     improvement.**
2. **[[dynamic-weight-sharing]]**
   - Done by adding **lateral connectivity** to a locally connected network and
     allowing learning via **Hebbian plasticity**.
   - This helps **equalise the weights** of the laterally connected neurons.
   - Lateral connections are added between **every k-th neuron** in the input
     and output layer.
   - A **sleep phase** is introduced every training iteration, where the weights
     of the laterally connected neurons `i` are updated by the "Hebbian rule":

```
Δw_i ∝ −( z_i − (1/N) Σ_{j=1}^{N} z_j ) x − γ ( w_i − w_i^init )
```

   - ⇒ **stochastic gradient descent on the sum `(z_i − z_j)²`**

### [[lime]] — Local Interpretable Model-Agnostic Explanations

- **Explain a classifier locally**, in the neighbourhood of a sample
- **Create a local dataset by perturbing** the sample and dropping
  interpretable units
- **Classify** these perturbed samples
- **Weigh** the perturbations by their similarity to the sample
- **Train an interpretable classifier** with this perturbed data

### Summary (p9, verbatim)

- Visual information processed in **different brain areas**
- Images **projected** from retina to striate and extrastriate cortex, filtered
  according to different features
- Visual processing inspired **hierarchical computational models**:
  object recognition (Fukushima, LeCun); deep learning models

## New pages created

Concepts — [[visual-pathway]], [[the-retina]], [[receptive-field]],
[[simple-complex-hypercomplex-cells]], [[orientation-tuning]],
[[two-visual-streams]], [[weight-sharing]], [[data-augmentation]],
[[dynamic-weight-sharing]], [[pooling]]

Systems — [[neocognitron]], [[locally-connected-network]], [[lime]]

Entities — [[david-hubel]], [[torsten-wiesel]], [[kunihiko-fukushima]],
[[yann-lecun]], [[leslie-ungerleider]]

## Pages updated

- **[[convolutional-network]]** — was a one-line stub from L03; now a full page.
- **[[dual-stream-hypothesis]]** — L04's dorsal/ventral swap **confirmed** by
  L06's *dorsal = where, ventral = what*.
- **[[ann-brain-correspondence]]** — [[dynamic-weight-sharing]] is the first
  concrete attempt in the module to make a learning mechanism biologically
  plausible.
- **[[hebbian-learning]]** — the rule finally appears **with a decay term**,
  five lectures after the unbounded-growth problem was raised.
- **[[network-architectures]]** — convolutional and locally-connected as
  architecture classes.
- **[[local-vs-distributed-representation]]** — ON/OFF centre-surround fields.
- **[[L04-embodied-language-processing]]** — the dorsal/ventral correction.

## Connections

- **The hierarchy correspondence is genuine.** Unlike *backpropagation ↔
  plasticity*, this one has documented lineage: Hubel & Wiesel's simple/complex
  cells → Fukushima's S-cells/C-cells → LeCun's conv/pool layers. Each step
  cites the last. Compare [[ann-brain-correspondence]].
- **The convolution stub is finally filled.** [[convolutional-network]] was named
  once in an L03 summary list and left as a stub for three lectures. L06 gives
  it equations, an architecture and a critique.
- **Hebb gets a decay term at last.** [[hebbian-learning]] has carried an
  unresolved "no remedy for self-amplification" complaint since L02. The
  `−γ(w_i − w_i^init)` term in [[dynamic-weight-sharing]] is exactly such a
  remedy — it pulls weights back towards their initialisation. First time in the
  module.
- **But it is called Hebbian and is not.** The update is
  `−(z_i − mean(z))x` — an *error-correcting* rule that drives a unit towards the
  population mean. This is the **third** time the module has labelled a
  non-Hebbian local rule "Hebbian" (after L04's SOM update, and the L04
  associator). The pattern is now clear enough to state: *the module uses
  "Hebbian" to mean "local", not "correlational".*
- **Plausibility becomes a research question, not an assertion.** L03 asserted
  that backprop corresponds to plasticity. L06 does the opposite: it identifies
  a specific mechanism CNNs need (weight sharing), states plainly that neurons
  cannot do it, and proposes two candidate repairs with measured results. This
  is the module's intellectual high point so far.

## Unclear in the source

- **`i ∈ [0, N]`** in the convolution formula should be `[0, N−1]`, since `N` is
  the *number* of patches. Off-by-one in the source.
- **The "Hebbian rule" on p9 is not Hebb's rule** — see above. It is a local
  error-correcting rule with a decay term.
- **The sign convention of the update is ambiguous.** `Δw_i ∝ −(z_i − mean)x`
  with a leading minus makes it gradient *descent* on `(z_i − z_j)²`, which
  matches the annotation. But the module's usual convention writes
  `w ← w + η·δ·…` with `δ = t − y` (see [[loss-function]]). Two sign conventions
  now coexist in the notes.
- **Why a "sleep phase"?** The term is introduced without motivation. `[external]`
  It is presumably an analogy to memory consolidation during sleep — running a
  separate offline phase in which weights are equalised without new input. The
  notes do not say.
- **`k` in "every k-th neuron"** is never given a value, and why lateral
  connections should skip neurons rather than connect all of them is not
  explained.
- **V4 is bracketed** — `(V4: Orientation)`. The parentheses suggest
  uncertainty. V4 is more usually associated with colour and form. `[external]`
- **V3 is given "colour, motion" and V5 "motion, eye movements"** — motion
  appears twice, and the V3/V5 division is not explained.
- **Amacrine, bipolar and horizontal cells are listed and never described.**
- **The ventral/dorsal RNN example is a sketch with no result.** The two
  questions it poses — can one RNN store multiple sensory sequences, can they
  still be encoded in two streams — are never answered.
- **LIME appears with no connection to the rest of the lecture.** No link is
  drawn between explainability and hierarchical vision, though the obvious one
  (what does a conv filter actually detect?) is right there.
- **LeNet's "Gaussian connections"** in the output layer are labelled and never
  explained.

## Verified from the source

These all check out:

- `N = (|x| − r)/s + 1` with `|x| = 8, r = 3, s = 1` gives `N = 6` — matching
  the `6` written on the diagram.
- Pooling with `r_p = 2, s_p = 2` takes `dim(z) = 8×2` to `dim(p) = 4×2`.
- The LeNet-5 chain `32×32 → 28×28 → 14×14 → 10×10 → 5×5 → 120 → 84 → 10` with
  6, 6, 16, 16 channels is correct.
- The horizontal-line kernel `(-1 -1 -1 / 2 2 2 / -1 -1 -1)` does detect
  horizontal lines, and sums to zero as an edge filter should.

See [[index]] · [[overview]] · [[log]]

## Revisited in L12

L12 restates this lecture's *edges ? parts ? objects* ladder verbatim, generalises
it to text and speech, and uses it to argue that deep networks are
**interpretable** rather than that they are **brain-like**. The two uses of one
observation are set out in [[levels-of-abstraction]].
