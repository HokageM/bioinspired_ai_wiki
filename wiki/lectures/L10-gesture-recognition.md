---
title: "L10 ? Neurally-Inspired Gesture Recognition"
type: lecture
lecture: L10
sources: [L10]
tags: [gesture, vision, robotics, recurrent, self-organisation, multimodal]
updated: 2026-09-21
---

# L10 ? Neurally-Inspired Gesture Recognition

Source: `raw/lectures/BioinspiredAIWdh10.pdf`, 12 pages, all marked `L10`.
Dated **13.02.2024** (pp3?4, 8?9) and **15.02.2024** (pp10?12).

The module's longest lecture, and the one that most resembles a research
programme: a single task ? recognise a gesture ? attacked with six successive
architectures, each introduced because the previous one failed in a stated way.

## What gestures are

> **A movement of a limb or the body as an expression of thought or feeling.**
> **But: in different cultures, different meanings.**

Relevance: **visual channel in multimodal communication**; interdisciplinary
(AI, psychology, neuroscience); **HCI** (input, navigating); **HRI** ? *passive:
responsive agent*; *active: learning of gestures / concepts*.

### The gesture continuum

> **Different abstraction levels in gesture meaning ? increasing level of
> linguistic properties and semantics.**

```
Gesticulation ? [ Iconic ? Deictic ] ? Pantomimes ? Emblems ? Sign language
   ?                                                      ?            ?
spontaneous:                                         e.g. "Peace"   communicative:
accompany body expression                                     supports or replaces speech
```

See [[gesture-continuum]].

## Towards vision-based recognition

**Data acquisition:** sensors ? state of hand, etc.

**Vision-based** is **most intuitive**: **(+) no cables ? (+) sign language ?
(+) communicating at distance.**

**Needs:** skeletal data, **depth sensors**, image processing.

**Issues:** (?) environmental influence: background, distance to sensors ?
(?) sensor choice: 2D, 3D, noise ? (?) single user / multiple users ? (?) task.

## Static and dynamic

| | **Static gestures** | **Dynamic gestures** |
|---|---|---|
| Definition | **static postures, no temporal information to be recognised** | **dynamic hand and arm movement and trajectories** |
| Example | *"OK"* | *"move to the left"* |
| Key | **hand shape and finger configuration** | **spatiotemporal pattern recognition** |
| Challenge | **postures with complex backgrounds** | **start and end of isolated or continuous gestures** |
| Models | template matching, elastic graph matching; **SVM, MLP, 2D/3D CNN** | probabilistic graphical models (HMMs); **RNN, SOM, growing-when-required networks** |

See [[static-and-dynamic-gestures]], [[gesture-representation]].

## The architectures, in order

1. **[[multichannel-cnn|MCCNN]]** ? 3D kernels over image stacks, Sobel channels,
   [[motion-history-image|MHI]].
2. **[[cnn-lstm]]** ? CNN for spatial features, LSTM for time.
3. **[[snapshot-model]]** ? static *and* dynamic channels in parallel.
4. **[[gwr-network|GWR]]** ? a self-organising map that grows.
5. **[[gamma-gwr]]** ? GWR plus recurrence, for temporal sequences.
6. **[[openpose]]** ? skeletal input, so the rest can be smaller.

Plus [[contrastive-language-image-pretraining]] for zero-shot labelling.

## The image-processing pipeline

```
Image sequence ? RGB to YCbCr ? skin regions / masks
               ? connected component analysis / labelling
               ? morphological operations ? threshold: hand
```

A classical, entirely hand-written pipeline ? [[functional-decomposition]] for
vision ? presented without comment immediately before the neural alternatives.

## Neural networks (recap)

`y = f(? w?x?)`. **Decision boundary**: how to capture it? ? **minimisation of
network error**; objective usually the mean-squared criterion

```
E_net = ? (y ? ?)?
```

**Learning = weight update.** `f` nonlinear for arbitrary shapes, continuous and
differentiable (e.g. sigmoid):

```
f(x) = 1 / (1 + exp(?x))
d/dx f(x) = f(x) ? (1 ? f(x))
```

**Error backpropagation w.r.t. weights** `?E_net/?w?` ? **gradient**.

> [!success] This closes an open thread from L03
> ~~No activation derivatives appear anywhere, though backpropagation requires
> them.~~ L10 finally gives one ? and the best one, since `f' = f(1?f)` is
> exactly why the sigmoid was chosen. See [[activation-function]].

## Deep networks: the honest balance sheet

**(+)** successful benchmarking in vision and audio ? robust feature emergence
due to hierarchical processing (*Entstehung*) ? optimisation efforts in open
software ? trends regarding transfer learning and generative/unsupervised models.

**(?)** **data-hungry and time-consuming** (exhaustive tuning, retraining,
optimisation strategies) ? **specialist for one task** (*adaptivity? feedback?*)
? **not fully appropriate for the temporal domain** ? **3D kernel: no
significant advantage over 2D kernels**.

See [[deep-network-tradeoffs]]. The last line is a **negative result**, and the
only one in the module.

## Errors and problems in the source

> [!warning] `E_net = ?(y ? ?)?` uses new, unexplained notation
> L03 wrote the error with `t` for target and `y` for output. Here it is `y` and
> `?`, and which is which is not stated. The module now has **three** sign and
> naming conventions for the same quantity ? see the note on
> [[perceptron-learning-rule]].

> [!warning] "Motion intensity profile" measures displacement from the first
> frame, not intensity
> [[motion-intensity-profile|ISSIM]] uses **the first frame of the sequence as
> reference**, so the curve is *dissimilarity from the rest position*, which
> accumulates and then returns. A genuine *intensity* profile would difference
> **consecutive** frames. The two agree only when the gesture is monotonic.
> The notes' own *"turn left"* example ? two peaks ? is consistent with either
> reading, so the figure does not disambiguate it.

> [!warning] Reservoir computing appears once, in the summary, and nowhere else
> *"Reservoir computing models by neurophysiological principles for action
> selection in prefrontal cortex and striatum."* No definition, no architecture,
> no mention anywhere in the preceding twelve pages. See
> [[reservoir-computing]].

> [!warning] The striatum arrives without the theory that requires it
> The summary names **prefrontal cortex and striatum** for **action selection**.
> The striatum is the module's first basal-ganglia structure, and it is the
> canonical site of reward-based learning. [[learning-paradigms]] has had
> reinforcement learning defined and unused since L03; L10 names the anatomy and
> still not the algorithm.

> [!warning] The classical pipeline is used without acknowledgement
> RGB?YCbCr, skin masks, connected components, morphology, threshold ? a
> hand-written vision pipeline, presented one page before a lecture-long
> argument for learned features, with no comment on the relationship.

## Unclear in the source

- **Kendon** is named for gesture phases with no first name, date or citation.
  See [[adam-kendon]].
- **"Iconic ? Deictic"** is bracketed inside the continuum without explanation of
  why those two are grouped.
- **The MCCNN classifier** is drawn but never specified.
- **`x?`, `x?`** in the CNN-LSTM diagram are unlabelled.
- **ChaLearn** is named as a benchmark with no description.
- **Gamma GWR's context descriptor** ? *"combination of previous BMU's weight and
  context vector"* ? the combination rule is not given, and it is the whole
  mechanism.
- **GWR's `?a_i = ?_i ? 1.05 ? (1 ? h_i) ? ?_i`** ? the constant `1.05` is
  unexplained, and the equation as written is dimensionally odd. See
  [[gwr-network]].
- **OpenPose's bipartite matching** is named, not defined.
- **No evaluation numbers anywhere** ? performance is described as *"good"*,
  *"decrease"*, *"robust"*.

## What this lecture adds to the module

**1. A network that changes its own architecture.** Everything before L10 learned
by changing **weights**. [[gwr-network|GWR]] adds and removes **nodes**, driven
by novelty and habituation. See [[gwr-network]].

**2. Recurrence inside a self-organising map.** [[gamma-gwr]] gives each node a
**context vector** so that previous winners influence the current one ? the RNN
trick, transplanted into competitive learning.

**3. A stated negative result.** *3D kernels give no significant advantage over
2D kernels.* The module otherwise reports only successes.

**4. Language?vision alignment.** [[contrastive-language-image-pretraining]] is
the module's first contrastive model and its first zero-shot system.

## Cross-references

- [[convolutional-network]] ? [[gated-recurrent-network|LSTM]] ?
  [[self-organising-map]] ? [[activation-function]]
- [[task-inference-network]] ? L08's *"self-organised network of behaviours"*;
  GWR is the obvious candidate and neither lecture says so
- [[embodied-language-representation]] ? gestures as the visual channel of
  language
