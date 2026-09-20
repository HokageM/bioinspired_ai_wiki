---
type: lecture
lecture: L07
title: Bio-inspired Crossmodal Processing
source: raw/lectures/BioinspiredAIWdh7.pdf
pages: 6
tags: [multimodal, neuroscience, integration, robotics]
---

# L07 — Bio-inspired Crossmodal Processing

> Source: `raw/lectures/BioinspiredAIWdh7.pdf`, 6 pages, all marked **L7**.

## Summary

L05 localised sound. L06 processed vision. L07 asks what happens when the two
disagree — and answers with the module's most complete piece of computational
neuroscience: a specific brain structure (the [[superior-colliculus]]), a set of
measured empirical signatures, a model, and an explicit check that the model
reproduces the signatures.

That last step is new. Previous lectures claimed correspondence by resemblance.
L07 validates by **behavioural and neurophysiological similarity** — the model is
judged by whether it exhibits [[spatial-principle|enhancement and depression]]
and [[inverse-effectiveness]], the same phenomena measured in real SC neurons.
See [[ann-brain-correspondence]].

## Key ideas

### Why integrate at all

**Modalities:** vision, audio, haptics, etc. Integrating them buys:

- **disambiguation of object properties**
- **increased accuracy, and dealing with redundancy in understanding events**

> **[[multisensory-integration]] (MSI) yields a coherent, robust and efficient
> interaction with the environment.**

The goal: **embedding of MSI in artificial agents and robots acting in the
world.**

### Terminology depends on discipline

| Term | Field | Meaning |
|---|---|---|
| **Crossmodal Integration** | interdisciplinary | behavioural tasks involving one or several senses |
| **Multimodal Integration** | AI, intelligent systems | integration of multiple knowledge-based modalities |
| **Multisensory Integration** | cognitive neuroscience | activity of neurons responding to one sense or more; neural processes involved in synthesising information from different stimuli |
| **Multisensory Fusion** | engineering, robotics | mapping several objects to a single object; combining information into a larger system |

> **MSI:** *the process by which information from different sensory modalities
> is combined to yield a rich, coherent representation of an object or event in
> the environment.*
>
> **More powerful than just using the most appropriate modality.**

That last line is the lecture's thesis, and it is a direct rejection of the
[[modality-appropriateness-hypothesis]] introduced on the next page.

### [[ventriloquism-effect]]

**Visual influence on auditory perception.** *Visual capture of a talking puppet
causes perception of sound direction to change.*

Three variants, all under the **[[modality-appropriateness-hypothesis]]**:

- **Spatial ventriloquism** — localisation at the position of the *visual* event
- **Temporal ventriloquism** — flash perceived closer in time than in reality
- **Double-flash illusion** — 2 flashes and 2 noise bursts perceived, although
  there was **only one flash**

> **Modality appropriateness hypothesis:** *that modality is given full credence
> which is most appropriate for the stimulus.* (margin: **Attention**)

Note the third example runs the other way: **sound alters vision.** So vision
does not simply win — whichever modality is better suited to the *dimension in
question* dominates. Space → vision. Time → audition.

### The alternative: integration

> **Alternative hypothesis: Integration.**
> *Small errors in visual localisation and large errors in auditory localisation
> lead to optimal localisation being very close to the visual estimate, hence the
> ventriloquism effect.*

This is [[optimal-cue-integration]], and it is a strictly better explanation: it
predicts the *same* outcome from a statistical principle rather than a stipulated
rule about which sense "wins", and it predicts the *degree* of capture rather
than just its direction. See that page for why the two hypotheses are not really
rivals.

### [[fusion-strategies]] in neural networks

**Simple types** — how two vectors are combined at a single point:

```
concatenation      multiplication      sum        function
  [a ; b]              a ⊙ b          a + b       f(a, b)
```

**Integration paths** — *where* in the network it happens:

| Strategy | Description |
|---|---|
| **a) Early fusion** | takes as input a **concatenated vector** |
| **b) Intermediate fusion** | **first representations of the data are learned, afterwards the modalities are fused** — can occur in one layer or gradually |
| **c) Late fusion** | **combines decisions by sub-models for each modality** |

### Where MSI happens in the brain

**Subcortical midbrain — the [[superior-colliculus]] (SC):**
*multisensory, localises stimuli, present in all vertebrates, motor output.*

**Cortical MSI — the superior temporal sulcus (STS)**, drawn between auditory
cortex and visual cortex.

**Bio-inspired MSI in the midbrain:**

- Primary input is **visual**, in addition to **auditory** and **somatosensory**
- Visual input → **LGN** → **SC**
- **SC is the main visual brain region**
- **SC sits right over the IC** (inferior colliculus — sound localisation)
- **SC is topographically organised**: depending on *where* a stimulus is, it
  generates activity in a **specific column** of the SC
- **Different sensory modalities go to different layers of the SC**

So the SC is a stack of maps of the same space, one per modality, in register
with each other — columns for *where*, layers for *which sense*.

### [[spatial-principle]] — SC neural responses

| Condition | Response |
|---|---|
| auditory only | baseline |
| visual only | baseline |
| **enhancement** | **large** — visual and auditory stimulus **close in time and space** |
| **depression** | **small** — visual and auditory stimulus **displaced from one another** |

### [[inverse-effectiveness]]

> **Weak stimuli enhance each other more strongly than strong stimuli.**

- **Strong intensity** ⇒ **subadditive** (combined < sum of parts)
- **Weak intensity** ⇒ **superadditive** (combined > sum of parts)

Both bar charts compare the measured bimodal response against a *theoretical*
bar, which is the sum of the two unimodal responses.

### [[unity-assumption]]

> **Whenever two or more sensory inputs are perceived as being highly
> consistent, observers will be more likely to treat them as referring to a
> single multisensory percept with a common spatiotemporal origin.**

```
consistent    →  unification    →  one multisensory percept
inconsistent  →  segregation    →  separate visual percept + auditory percept
                 ("prior entry")
```

### Neural phenomena, restated probabilistically

- **Depression:** *more probable response to only-visual than response to both
  (crossmodal).*
- **Enhancement:** *more probable response to cross-modal than response to
  only-visual.*

### [[self-organising-map]] for multimodal integration

The SOM returns from L04, now doing integration rather than clustering:

```
┌─ SC ─────────────────────────────┐   ┌──────────────────────┐
│  Superficial layers:             │   │ Inferior Colliculus: │
│  visual localisation      → x_v  │   │ auditory loc.  → x_a │
│                             │    │   │              │       │
│                             ▼    │◄──┴──────────────┘       │
│  Deeper integration layer:       │
│  (SOM)(x_v, x_a)          → x_s  │
└──────────────────────────────────┘
```

> **The SOM learns which representations cluster together. It could learn map
> registration.**

*Map registration* — aligning the visual and auditory maps of the same space so
that "straight ahead" means the same column in both — is the thing that has to
be true for the spatial principle to work at all. The lecture raises it as a
possibility and does not pursue it.

### [[histogram-based-som]]

> **Implementation as histogram-based SOM** → *novel SOM with probabilities.*

- **Histograms for each input dimension** instead of single-valued prototypes
- Visual input and auditory input → **divisive normalisation** → **PDF**
  (*population-coded probability density function*)
- **Output: likelihood of input given histograms**
- **Training like SOM, but: update histograms, not single-valued prototypes**

> **⇒ The ANN model combines self-organisation and statistical integration.**
> NN integrates, compatible with the **ML estimator model**.
> **The model reproduces the same phenomena as the biological SC:** depression,
> spatial principle; enhancement, inverse effectiveness; MSI.

### From SC to a cortico-collicular arch

- **Top-down: cortical.** **Bottom-up: collicular (subcortical) alignment.**
- **Multimodal units show the highest activation for congruent audiovisual
  patterns.**
- That activity is used as a **top-down modulatory projection to bias the
  integration at the subcortical layer.**

See [[cortico-collicular-architecture]] and [[top-down-modulation]].

### [[gated-multimodal-unit]] — integration at unit level

- **Information fusion: creates a new representation out of different
  modalities.**
- **Information flow is controlled by gate neurons `σ`, that determine modality
  contribution.**
- **Fully differentiable NN unit.**

```
        x_v              x_t
         │        σ ◄─────┤
         ▼        │       ▼
      tanh        │     tanh
         │        │       │
         ▼        ▼       ▼
        (×σ)          (×(1−σ))
           └─────(+)─────┘
                  │
                  ▼
                  z
```

### Summary (p6, verbatim)

- Models for bio-inspired sound source localisation, visual localisation and
  multisensory integration
- **Effectiveness of bio-inspired models**
- **Demonstrated behavioural, neurophysiological similarity between models and
  biological systems**
- Levels of neural activity in the unimodal layer may provide the **reliability
  of each modality**
- Multimodal neurons exhibiting the highest activation for congruent audiovisual
  patterns
- Activity-driven levels of congruency used as **top-down modulatory
  projections** to bias the integration at the subcortical layer
- **Inverse effectiveness**
- **Superior Colliculus ⇒ MSI in brain**

## New pages

Concepts — [[multisensory-integration]], [[ventriloquism-effect]],
[[modality-appropriateness-hypothesis]], [[optimal-cue-integration]],
[[fusion-strategies]], [[superior-colliculus]], [[spatial-principle]],
[[inverse-effectiveness]], [[unity-assumption]], [[top-down-modulation]]

Systems — [[histogram-based-som]], [[gated-multimodal-unit]],
[[cortico-collicular-architecture]]

**No people are named anywhere in this lecture** — unusual for the module, and
notable given how much specific experimental data it reports.

## Connections

- **The SC was named in L06 and is developed here.** [[L06-hierarchical-vision]]
  listed the superior colliculus as one of four subcortical targets of the
  retina, function: *eye movements*. L07 makes it the centrepiece. Good
  continuity, and neither lecture cross-references the other.
- **A new validation standard.** *Demonstrated behavioural, neurophysiological
  similarity between models and biological systems* — the model is checked
  against measured phenomena rather than declared similar. This is the third and
  strongest standard in the module. See [[ann-brain-correspondence]].
- **The gate is the GRU gate.** The [[gated-multimodal-unit]]'s
  `z = σ·tanh(·) + (1−σ)·tanh(·)` is precisely the convex-combination gate of
  L03's [[gated-recurrent-network]], applied **across modalities** instead of
  **across time**. Neither lecture mentions the other.
- **Topographic code, sixth instance.** *SC is topographically organised:
  depending on where a stimulus is, it generates activity in a specific column.*
  See [[place-cells]] and [[overview]].
- **The SOM's third job.** L04 used it to cluster word meanings, L07 uses it to
  fuse modalities. See [[self-organising-map]].
- **L05's loose ends are half-addressed.** The [[cocktail-party-problem]] is
  still not solved, but [[azimuth-and-elevation]]'s missing second coordinate is
  implicitly handled — the SC is a 2D map. The lecture does not say so.

## Unclear in the source

- **The two ventriloquism hypotheses are never adjudicated.** Modality
  appropriateness and integration are presented as alternatives, but the second
  *explains* the first — see [[optimal-cue-integration]]. The notes leave them
  side by side.
- **Inverse effectiveness and optimal integration are never connected**, though
  they are the same idea seen from two directions: an unreliable cue has more to
  gain from being combined. Two pages apart, no link drawn.
- **No equations anywhere for MSI itself.** The ML estimator model is invoked by
  name on p5 and never written down, despite being the lecture's explanatory
  core.
- **"Divisive normalisation" is named and not defined.**
- **The gate `σ` in the GMU:** the diagram shows a single `σ` fed by both `x_v`
  and `x_t`, but whether `σ` is a scalar or a vector (per-feature gating) is not
  stated, and no formula is given.
- **"Prior entry"** appears once, undefined, attached to segregation.
- **No people, dates or citations**, despite reporting specific experimental
  results (the double-flash illusion, SC recordings).
- **"SC is the main visual brain region"** is a strong claim that sits oddly
  beside L06, which gave that role to V1 and the occipital lobe. Possibly means
  *main visual region of the midbrain*. `[external]` In primates the SC is
  chiefly an orienting/saccade structure; V1 dominates visual processing.
- **Map registration is raised and dropped** — *"it could learn map
  registration"* — without any experiment or result.

See [[index]] · [[overview]] · [[log]]
