# Log

Append-only, newest at the bottom. Entry headers follow
`## [YYYY-MM-DD] <type> | <subject>` so `grep "^## \[" wiki/log.md | tail -5` works.

See [[index]] for the page catalogue and [[overview]] for the module map.

## [2026-09-20] refactor | Wiki initialised
Created: CLAUDE.md schema, directory scaffold, index.md, log.md
Notes: Scope set to bio-inspired AI broadly (neuroscience, evolution, swarm/collective
intelligence and their ML analogues). Primary sources will be the human's own summaries
of lectures 1–13 of the Bio-Inspired AI module, dropped into `raw/lectures/`.

## [2026-09-20] refactor | Schema updated for scanned PDF sources
Updated: CLAUDE.md (§2 layout, new §2.1 reading workflow, §3 pseudocode rule)
Notes: Sources turned out to be photographed handwritten notes as PDFs with no text
layer. Added the pymupdf render-to-JPEG workflow, the `Wdh` filename caveat (files are
revision numbers, not lecture numbers — `Wdh1und2` covers two lectures), and the
highlighter-colour legend. Made pseudocode a standing requirement on every page with
algorithmic content, per the human's instruction.

## [2026-09-20] ingest | L01 Introduction + L02 Spiking Neural Networks
Source: raw/lectures/BioinspiredAIWdh1und2.pdf (8 pages, handwritten, 18–19.01.2024)
Created lectures: L01-introduction-to-bio-inspired-ai, L02-spiking-neural-networks
Created concepts: intelligent-behaviour, place-cells, grid-cells, action-potential,
  refractory-period, excitatory-and-inhibitory-neurons, neural-coding, rate-coding,
  temporal-coding, activation-function, neural-similarity-and-dot-product,
  linear-separability, local-vs-distributed-representation, network-architectures,
  synaptic-plasticity, hebbian-learning, stdp, synaptic-kernel
Created systems: mcculloch-pitts-neuron, spiking-neural-network,
  discrete-dynamic-neuron, continuous-dynamic-neuron, integrate-and-fire,
  spike-response-model, hodgkin-huxley-model
Created entities: donald-hebb
Updated: index, log, overview (created)
Notes: 28 pages from one source. L01 is one page of framing; L02 is seven pages and
  carries all the technical content.
  - FLAGGED as a probable note-taking error: the notes equate the logistic sigmoid with
    tanh. Recorded on activation-function and in L02's "Unclear in the source".
  - FLAGGED notation drift: integrate-and-fire mixes u_i and a_i for the same variable.
  - GAP: linear separability is established but its consequence (XOR / why multilayer)
    is never stated in the notes.
  - GAP: hodgkin-huxley-model is named once and never explained — left as a stub.
  - Stubs created deliberately for grid-cells, hodgkin-huxley-model and donald-hebb,
    each recording exactly what the source does and does not say.
  - Noted in index: there is no Wdh9.pdf among the raw files.

## [2026-09-20] ingest | L03 Computational Neural Networks
Source: raw/lectures/BioinspiredAIWdh3.pdf (6 pages, handwritten, 25-26.01.2024)
Created concepts: levels-of-abstraction, perceptron-learning-rule,
  perceptron-convergence-theorem, xor-problem, loss-function,
  overfitting-and-underfitting, regularisation, dropout, batch-vs-online-training,
  vanishing-gradient-problem, learning-paradigms, ann-brain-correspondence
Created systems: multi-layer-perceptron, backpropagation, recurrent-neural-network,
  simple-recurrent-network, gated-recurrent-network, autoencoder, convolutional-network
Updated: linear-separability, mcculloch-pitts-neuron, activation-function,
  network-architectures, hebbian-learning, index, overview
Notes: 20 pages created, 6 revised.
  - RESOLVED an L02 gap: linear-separability's missing consequence is L03's XOR problem.
    Both that page and mcculloch-pitts-neuron (which lacked a learning rule) updated.
  - FLAGGED a cross-lecture tension on ann-brain-correspondence: L03 asserts
    "backpropagation <-> plasticity", but L02's biological rules (Hebb, STDP) are local
    while backprop is global. L03 itself concedes the point when it calls the SRN
    "biologically more plausible than an MLP".
  - FLAGGED: the regularisation penalty is written as lambda*sum(w_j) - signed and
    un-squared, so not a norm. Probably a dropped square or abs.
  - FLAGGED: LSTM and GRU are conflated (input/forget/output gates, then "update and
    reset gate" with no separation).
  - FLAGGED: eta = 1/35 appears unexplained; page 3 is dated 2023 while pages 4-6 are 2024.
  - NOTE: BioinspiredAIWdh9.pdf IS present (it was absent at the previous listing).
    The "missing source" warning has been removed from index.
  - GAP carried forward: no activation derivatives anywhere, though backprop needs them.

## [2026-09-20] ingest | L04 Bio-inspired Embodied Language Processing
Source: raw/lectures/BioinspiredAIWdh4.pdf (10 pages, handwritten, 26.01 + 31.01.2024)
Created concepts: compositionality-of-language, phrenology, language-areas-of-the-brain,
  dual-stream-hypothesis, embodied-language-representation, symbol-grounding,
  transfer-learning, word-embedding, self-supervised-learning,
  cross-modal-stimuli-prediction
Created systems: self-organising-map, multi-layer-associator, word2vec, gpt, hubert,
  imitation-network
Created entities: paul-broca, carl-wernicke, teuvo-kohonen
Updated: hebbian-learning, learning-paradigms, local-vs-distributed-representation,
  network-architectures, ann-brain-correspondence, gated-recurrent-network,
  index, overview
Notes: 20 pages created, 6 revised. Wiki now 71 pages.
  - First lecture to leave the single neuron behind. Subject is symbol grounding,
    though the notes never use the phrase.
  - FLAGGED: dorsal/ventral are almost certainly swapped. The notes label comprehension
    "dorsal" and production "ventral"; the standard dual-stream account is the reverse.
    Worth checking against the original slides.
  - FLAGGED: "Sparseness: 25x25 cells each layer" conflates layer size with sparseness.
  - FLAGGED: the SOM update w <- w + h*eta*(x-w) is called Hebbian by the lecture but
    contains a decay term -h*eta*w, which is precisely what the bare Hebb rule lacks.
    Recorded on hebbian-learning and self-organising-map.
  - FLAGGED: "avg min_ij d_ij" on p5 is almost certainly "arg min".
  - FLAGGED: GPT's pre-training is called "unsupervised" while HuBERT's is called
    "self-supervised", for structurally identical setups. New page
    self-supervised-learning treats them as one paradigm and notes the drift.
  - RESOLVED two standing gaps: learning-paradigms had unsupervised learning defined
    but no algorithm (now the SOM), and network-architectures had no attention-based
    architecture (now GPT).
  - NEW TENSION, recorded on ann-brain-correspondence: across L04's five models, none
    is both locally-learning and embodied, and the two that perform best on real
    language are neither. The lecture argues for grounding for seven pages, then
    presents GPT without comment.
  - GAP: reinforcement learning is still named-only, three lectures on.
  - GAP: evolution and swarm intelligence still absent, four lectures in.

## [2026-09-20] ingest | L05 Bio-inspired Robot Sound Localisation
Source: raw/lectures/BioinspiredAIWdh5.pdf (6 pages, handwritten, 02.02.2024)
Created concepts: azimuth-and-elevation, interaural-time-difference,
  interaural-level-difference, acoustic-shadow, geometric-sound-localisation,
  cocktail-party-problem, tonotopic-representation, auditory-pathway,
  hybrid-architecture
Created systems: jeffress-model, cross-correlation-localisation,
  hybrid-acoustic-tracking, hybrid-spiking-localisation-network
Created entities: lloyd-jeffress
Updated: spiking-neural-network, temporal-coding, simple-recurrent-network,
  neural-similarity-and-dot-product, intelligent-behaviour,
  ann-brain-correspondence, network-architectures, index, overview
Notes: 15 pages created, 7 revised. Wiki now 86 pages.
  - L02'S DEBT IS PAID. Spiking neurons were built in L02 and then unused through two
    lectures of rate-coded networks. ITD is tens of microseconds; no firing rate
    resolves that, so the code MUST be temporal. First task in the module that
    requires spikes rather than merely permitting them.
  - THE BEST CORRESPONDENCE SO FAR, recorded on ann-brain-correspondence: the Jeffress
    delay-line array and the cross-correlation algorithm are the SAME computation.
    The algorithm iterates over delays; the brainstem instantiates them in parallel.
    Both sides are fully written down and can be compared - which is exactly what
    "backpropagation <-> plasticity" (L03) never was. New standard proposed on that
    page: a correspondence is worth something when both sides can be written down.
  - NEW THEME: hybrid-architecture. First lecture to argue for NOT learning something.
    Use an algorithm where the physics is known (arccos), learning where the
    uncertainty actually is (the source's future trajectory).
  - RECURRING PATTERN now named on tonotopic-representation: four instances of
    "continuous quantity -> position in an ordered population" (place cells L01,
    SOM units L04, tonotopy L05, coincidence detectors L05). Never stated as a
    general principle by any lecture; arguably the module's most consistent
    representational commitment.
  - VERIFIED the worked example on p2: d=8, r=32000, c=340 -> a=0.085 m;
    arccos(0.085/0.15) = 55.48 deg. Both correct.
  - FLAGGED: the arccos convention is unstated - it measures from the MICROPHONE AXIS,
    not from straight ahead. A broadside convention would use arcsin.
  - FLAGGED: AVCN and MNTB are drawn in the p6 circuits and never defined. MNTB's
    sign-inversion is what lets the LSO subtract; without it there is no ILD at all.
  - FLAGGED: cross-correlation step 1 sets d=|g| (out of range, a sentinel), and
    "s_i >= s" biases tie-breaks towards the most negative shift. No normalisation.
  - FLAGGED: SRN layer sizes 45:30 / 30:45 / 30:30 are ambiguous; why an angle needs
    45 input units is unexplained (4-degree bins over 180 would fit).
  - GAP: elevation is introduced on p1 and every model solves azimuth only.
    Front/back ambiguity never mentioned either.
  - GAP: the cocktail party problem opens the lecture; every system assumes ONE source.
  - GAP: evolution and swarm intelligence still absent, five lectures in.

## [2026-09-21] ingest | L06 — Bio-inspired Hierarchical Vision (BioinspiredAIWdh6.pdf, 9pp)

All 9 pages marked **L6**, dated 06.02.2024. Rendered to JPEG and read visually;
no text layer. Topic: retina → V1 → hierarchy → Neocognitron → CNN, then a
sustained critique of the CNN's biological plausibility.

**Created (19).**
Lecture: `L06-hierarchical-vision`.
Concepts (10): `visual-pathway`, `the-retina`, `receptive-field`,
`simple-complex-hypercomplex-cells`, `orientation-tuning`, `two-visual-streams`,
`weight-sharing`, `data-augmentation`, `dynamic-weight-sharing`, `pooling`.
Systems (3): `neocognitron`, `locally-connected-network`, `lime`.
Entities (5): `david-hubel`, `torsten-wiesel`, `kunihiko-fukushima`,
`yann-lecun`, `leslie-ungerleider`.

**Updated (13).** `convolutional-network` (stub → full page),
`dual-stream-hypothesis`, `L04-embodied-language-processing`,
`ann-brain-correspondence`, `hebbian-learning`, `network-architectures`,
`local-vs-distributed-representation`, `neural-similarity-and-dot-product`,
`excitatory-and-inhibitory-neurons`, `learning-paradigms`, `auditory-pathway`,
`levels-of-abstraction`, `place-cells`, `tonotopic-representation`,
`neural-coding`.

**RESOLVED** — `convolutional-network` was a stub carrying the lint note *check
whether a later lecture develops convolutional networks*. L06 develops them
fully, three lectures later.

**RESOLVED** — the L04 dorsal/ventral flag. L06 p4 gives *dorsal = where,
ventral = what*; L04 labelled comprehension (meaning = *what*) as dorsal. The
module contradicts itself and L06 is correct. This is the first error confirmed
**from inside the module** rather than against external reference.

**RESOLVED (partially)** — the standing lint question on
`ann-brain-correspondence`: *does any later lecture offer a biologically
plausible alternative to backpropagation?* L06 offers one for **weight
sharing** (`dynamic-weight-sharing`), not for credit assignment through depth.
Row 3 of the L03 table still stands unsupported.

**RESOLVED** — Hebb's self-amplification problem, flagged in orange on L02 p7 and
carried unresolved through L04. L06's sleep-phase rule contains
`−γ(w_i − w_i^init)`, the module's first weight-decay term. Five lectures.

**VERIFIED** — checked and correct: `N = (|x|−r)/s + 1` gives 6 for
`|x|=8, r=3, s=1`; pooling `8×2 → 4×2` with `r_p=s_p=2`; the full LeNet-5
dimension chain; the horizontal-line kernel sums to zero; and the claim that
`Δw_i ∝ −(z_i − z̄)x` is gradient descent on `Σ(z_i − z_j)²` — it is.

**FLAGGED** — `i ∈ [0, N]` in the convolution formula should be `[0, N−1]`.

**FLAGGED** — third instance of a non-Hebbian rule called "Hebbian" (after L04's
SOM and associator). Now recorded on `hebbian-learning` as a finding about the
module's vocabulary: **"Hebbian" is used to mean *local*, not *correlational*.**

**FLAGGED** — `w_i^init` is ambiguous between *weights at training
initialisation* and *weights at the start of the current sleep phase*. The
marginal annotation supports the latter; the wiki takes that reading and says so.

**FLAGGED** — two sign conventions for weight updates now coexist in the notes.

**NEW TENSION** — L06 asserts rate coding for ganglion cells without argument,
one lecture after L05's Jeffress model made the strongest possible case for
temporal coding. Recorded on `neural-coding`.

**PATTERN (5th instance)** — the ordered-population code:
`orientation-tuning` joins `place-cells`, SOM units, `tonotopic-representation`
and the Jeffress detectors. Five lectures, still unnamed by the module.

**PATTERN (3rd instance)** — the dot product as similarity: L02's neuron, L05's
cross-correlation, L06's convolution. The last two are the *same expression*.
Also the third time normalisation is silently omitted.

**GAP** — L06 gives no training procedure for the CNN at all; no performance
figures for `dynamic-weight-sharing`; `lime` appears with no connection drawn to
the rest of the lecture.

**GAP (standing)** — evolution and swarm intelligence remain entirely absent six
lectures into a module named for them. Reinforcement learning still named-only.
No learning rule for spiking networks.

## [2026-09-21] ingest | L07 — Bio-inspired Crossmodal Processing (`BioinspiredAIWdh7.pdf`, 6 pp)

**Created (14):** `L07-crossmodal-processing`; concepts `multisensory-integration`,
`ventriloquism-effect`, `modality-appropriateness-hypothesis`,
`optimal-cue-integration`, `superior-colliculus`, `spatial-principle`,
`inverse-effectiveness`, `unity-assumption`, `fusion-strategies`,
`top-down-modulation`; systems `histogram-based-som`, `gated-multimodal-unit`,
`cortico-collicular-architecture`.

**Updated (10):** `self-organising-map`, `gated-recurrent-network`,
`ann-brain-correspondence`, `cross-modal-stimuli-prediction`, `visual-pathway`,
`auditory-pathway`, `network-architectures`, `place-cells`,
`local-vs-distributed-representation`, `hybrid-architecture`.

**NEW STANDARD.** L07 validates a model by showing it **reproduces measured
biological signatures** (enhancement, depression, spatial principle, inverse
effectiveness) — strictly stronger than L03's assertion or L05/L06's
written-down mappings. Recorded on `ann-brain-correspondence`. Caveat: the notes
state the result and show no data.

**PATTERN.** The GMU gate `σ·tanh(·) + (1−σ)·tanh(·)` is **exactly** the L03 GRU
update gate, applied across modalities instead of time. Neither lecture mentions
the other. Recorded on both pages.

**PATTERN.** Ordered population code, **6th instance**: SC topographic columns —
and the first time the maps are *stacked in register*. Still unnamed by the module.

**FLAGGED.** Two ventriloquism explanations (modality appropriateness vs optimal
cue integration) presented and never adjudicated — though they are not rivals:
appropriateness is the limiting case of optimality. **Inverse effectiveness is
never connected to optimal cue integration** although it is the same idea.
"Divisive normalisation" and "prior entry" named, not defined. GMU `σ`
scalar-or-vector unstated. "SC is the main visual brain region" conflicts with
L06's V1-centred account.

**GAP.** Zero people, dates or citations anywhere in L07 — unique in the module;
no entity pages created. **Evolution and swarm intelligence still entirely absent,
seven lectures into a module named for them.**

## [2026-09-21] ingest | L08 — Behaviour-based Robotics (`BioinspiredAIWdh8.pdf`, 10 pp)

**Created (17):** `L08-behaviour-based-robotics`; concepts
`functional-decomposition`, `reactive-agent`, `embodiment-and-situatedness`,
`behaviour-coordination`, `motor-schema`, `potential-field-navigation`,
`local-minima-problem`, `imitation-learning`, `uncanny-valley`; systems
`braitenberg-vehicle`, `subsumption-architecture`, `nico`,
`object-picking-architecture`, `neural-grasp-learning`, `task-inference-network`;
entities `rodney-brooks`, `valentino-braitenberg`.

**Updated (10):** `intelligent-behaviour`, `learning-paradigms`,
`fusion-strategies`, `hybrid-architecture`, `data-augmentation`,
`imitation-network`, `backpropagation`, `top-down-modulation`,
`excitatory-and-inhibitory-neurons`, `self-organising-map`.

**MAJOR CONTRADICTION — the lecture against itself.** p1 rejects functional
decomposition (perception→modelling→planning→execution) as brittle, expensive
and subject to the granularity problem. p9 presents imitation learning as
*Human Demo → Symbolic Reasoning → Action Plan → Inverse Kinematics → Robot
Imitation* — the same four stages — with no comment. p8's *modular vs end-to-end*
spectrum is the same axis a third time. Flagged on three pages.

**PATTERN — the combine/arbitrate fork, twice.** L07's `fusion-strategies`
(sum vs max vs learned function) and L08's `behaviour-coordination` (motor
schemas vs action selection) are the same decision, once for sensory input and
once for motor output, named differently. Only the sensory side has a *learned*
join (the GMU); every gain in L08 is hand-set.

**PATTERN — gradient descent, made physical.** Potential-field navigation is
L03's descent with position substituted for weights; local minima, cycling and
the noise remedy all carry over exactly.

**FLAGGED.** `V_magnitude = ∞` for `d ≤ R` is discontinuous and unimplementable
inside a linear combination. `S`, `B`, `G` cannot all have length `n`. `*` in
`ρ = C(G * B(S))` undefined. Suppression *timing* — the hardest part of
subsumption — never mentioned. The grasp-learning cycle can only generate data
from configurations the robot can already reach and release. "Uncanny valley"
named, never defined.

**VERIFIED.** Both piecewise schema functions are continuous at their inner
boundary (Guarded at `d=S` → `G`; Avoid-Obstacle at `d=S` → `0`). ✓ Straight
wiring really does turn the vehicle *away* from the light on a differential
drive. ✓

**GAP.** Reinforcement learning still absent — in the first lecture with an
agent, actions and outcomes, whose own disadvantage list names the problem
(*"difficult to make a reactive agent that learns globally"*). **Evolution and
swarm intelligence still entirely absent, eight lectures in.** Haptics arrives as
a fourth modality with none of L07's integration theory applied to it.

## [2026-09-21] ingest | L09 — Bio-inspired Attention (`BioinspiredAIWdh9.pdf`, 7 pp, dated 10.07.2024)

**Created (17):** `L09-bio-inspired-attention`; concepts `attention`,
`exogenous-and-endogenous-attention`, `attention-networks`, `reaction-time`,
`additive-factors-method`, `pop-out-effect`, `feature-integration-theory`,
`saliency-map`, `winner-take-all`, `auditory-scene-analysis`, `social-attention`;
systems `attention-network-test`, `saliency-model`, `auditory-attention-model`,
`human-robot-collaboration`, `nao`, `icub`.

**Updated (11):** `gpt`, `ventriloquism-effect`, `top-down-modulation`,
`ann-brain-correspondence`, `the-retina`, `orientation-tuning`,
`hybrid-architecture`, `self-organising-map`, `cross-correlation-localisation`,
`local-vs-distributed-representation`, `learning-paradigms`.

**RESOLVED — "attention is used but never defined"** (open since L04). L09
defines it fully. **But it is a different thing**: psychological attention is
capacity-limited, serial and discards the losers; transformer self-attention is
none of those. The module uses one word for two mechanisms and never puts them
side by side. Logged on `attention`, `gpt` and `ann-brain-correspondence` as a
**new failure mode — named alike, unrelated**.

**RESOLVED — the cocktail party problem** (open since L05). `auditory-scene-
analysis` gives the shape of the answer: group → segregate → compete, with
top-down bias at every stage. Architecture, not algorithm — and ITD, which L05
computes, is the obvious grouping cue and is not mentioned.

**PATTERN — winner-take-all, 5th instance.** SOM best-matching unit (L04), max
fusion (L07), action selection and subsumption (L08), saliency and auditory
object competition (L09). L09 is the first to name it as an algorithm. Its
biological appeal is that lateral inhibition implements argmax **locally**.

**PATTERN — top-down modulation, 3rd and strongest.** L07 biased one stage, L08
one stage, L09 **every** stage — including *grouping*, which means what counts
as one sound depends on what you are listening for.

**FLAGGED.** ASA's text and diagram give **opposite stage orders** (text:
segregate-then-group; diagram: group-then-segregate). The three ANT scores have
inconsistent polarity. Additive factors is used in reverse (affirming the
consequent). "Cortical Oscillation Model" is a heading with no content. V1
firing rate claimed monotonic in salience, which is in tension with tuning.
Saliency "extended to familiarity and threat" breaks FIT's preattentive stage.
**Ventriloquism now has three unadjudicated explanations** (L07 ×2, L09 ×1) —
and the third is experimentally distinguishable from the first two.

**GAP.** Reinforcement learning absent for a sixth consecutive lecture, in a
lecture about allocating a limited resource to maximise task performance.
**Evolution and swarm intelligence still entirely absent, nine lectures in.**
Social-attention results reported with no numbers, no statistics, no citation.


## [2026-09-21] ingest | L10 — Neurally-Inspired Gesture Recognition (`BioinspiredAIWdh10.pdf`, 12 pp)

**Created (18):** `lectures/L10-gesture-recognition`; concepts
`gesture-continuum`, `static-and-dynamic-gestures`, `gesture-representation`,
`gesture-phases`, `motion-intensity-profile`, `motion-history-image`,
`human-pose-estimation`, `deep-network-tradeoffs`, `reservoir-computing`;
systems `multichannel-cnn`, `cnn-lstm`, `snapshot-model`, `gwr-network`,
`gamma-gwr`, `openpose`, `contrastive-language-image-pretraining`; entity
`adam-kendon`.

**Updated (13):** `activation-function`, `perceptron-learning-rule`,
`self-organising-map`, `task-inference-network`, `convolutional-network`,
`gated-recurrent-network`, `learning-paradigms`, `hybrid-architecture`,
`embodied-language-representation`, `word2vec`, `data-augmentation`,
plus `index.md` and `overview.md`.

**RESOLVED** — activation derivatives. Open since L03: the module used gradient
descent without ever stating the derivative of an activation function. L10's
recap gives `f'(x) = f(x)(1 − f(x))` for the sigmoid, so the chain rule is now
completable from the module's own material. `tanh`, ReLU and softmax derivatives
remain absent.

**RESOLVED (inferred)** — L08's *"self-organised network of behaviours"* is
almost certainly GWR: same research programme, same stated problem (fixed task
representations block continual learning), same solution. Neither lecture says
so; recorded as inference, not fact.

**PATTERN** — first structural plasticity in the module. Every learning rule
through L09 changes **weights** in a fixed architecture. GWR changes **which
units and connections exist**. Gamma-GWR then adds recurrence inside competitive
learning — a memory with no gradient anywhere.

**PATTERN** — trade time for space, again. [[motion-history-image]] encodes
*when* as *how bright* so a memoryless convolution can read a trajectory,
exactly as L05's [[jeffress-model]] encoded a time difference as a place. Third
instance of the module's most reliable engineering move.

**PATTERN** — ordered-population code, instance seven, and the most abstract:
[[openpose]]'s **part affinity fields** lay out *pairwise association* in the
image plane. The coded variable is a binding, not a stimulus property. It is also
[[auditory-scene-analysis|Gestalt grouping]] from L09 done in vision by a learned
field — consecutive lectures, no connection drawn.

**PATTERN** — winner-take-all, instance eight: CLIP's zero-shot argmax over a
vocabulary of sentences.

**FLAGGED** — the *motion intensity profile* references the **first frame**, so
it measures cumulative displacement from rest, not intensity of motion. The two
differ exactly during a **hold**, which is the phenomenon the method is
introduced to analyse. Peak detection works either way, which is presumably why
this went unnoticed.

**FLAGGED** — `E_net = ½(y − ȳ)²` is the module's **third** notation for the
same error, and the overbar denotes a *target* here after denoting a *mean* in
L05.

**FLAGGED** — MCCNN's Sobel channels are hand-specified edge detectors inside a
network whose premise is learned features, and L06 argued that a trained first
layer **discovers** exactly those. Learned-vs-specified, now inside one layer.

**FLAGGED** — CLIP is the strongest available test case for L04's grounding
question and arrives on a gesture-recognition slide with no reference to it.

**GAP** — reinforcement learning, named-only since L03, is now conspicuous. L10
names the **striatum** and **action selection** — the anatomy and the function —
and still gives no reward, value function or policy. Four lectures have now
reached action selection by different routes without converging.

**GAP** — evolution and swarm intelligence remain **entirely absent** through
L10, in a module called Bio-Inspired AI. Three lectures left.

**GAP** — no evaluation numbers anywhere in L10: *"data-hungry"*, *"no
significant advantage"*, *"classification boost"*, all without a benchmark.

**Wiki state:** 173 pages, 0 dead links, 0 orphans.


## [2026-09-21] ingest | L11 — Evolutionary Computing (`BioinspiredAIWdh11.pdf`, 11 pp)

**Created (24):** `lectures/L11-evolutionary-computing`; concepts
`four-pillars-of-evolution`, `genotype-and-phenotype`, `dna-and-heredity`,
`candidate-representation`, `genetic-neural-encoding`, `fitness-function`,
`fitness-landscape`, `parent-selection`, `survivor-selection`, `recombination`,
`mutation`, `selection-pressure`, `premature-convergence`,
`competing-conventions-problem`, `inverse-kinematics`; systems
`evolutionary-algorithm`, `fitness-proportional-selection`, `ranking-selection`,
`roulette-wheel-selection`, `tournament-selection`, `neuroevolution`,
`genetic-inverse-kinematics`, `collision-free-navigation`.

**Updated (10):** `ann-brain-correspondence`, `learning-paradigms`,
`backpropagation`, `local-minima-problem`, `winner-take-all`,
`braitenberg-vehicle`, `embodiment-and-situatedness`, `deep-network-tradeoffs`,
plus `index.md` and `overview.md`.

**RESOLVED** — **the module's longest-standing gap**. Evolution has been flagged
as absent at every ingest since L01. L11 is evolution in full: biology, scheme,
every operator, two applications. Swarm and collective intelligence remain absent
with two lectures left.

**PATTERN** — bio-inspiration **one level up**. *"Brain: 'wheel' → evolutionary
mechanism, that created the human brain."* Ten lectures copy the brain; L11 copies
the process that produced it. Recorded as a fifth standard on
`ann-brain-correspondence`, and the most defensible one in the module — the four
pillars are substrate-neutral, so this is not a metaphor for evolution, it is
evolution on another substrate.

**PATTERN** — winner-take-all, instance nine, with a twist. A tournament is
argmax over a **random subset**, and `k` therefore tunes greediness. Every earlier
instance runs argmax over a fixed set. Subsampling the competition to soften the
competition is a general trick the module never states.

**PATTERN** — structure search twice in consecutive lectures by opposite means:
GWR adapts capacity to the **data**, online; neuroevolution adapts it to the
**task**, across generations. Neither lecture references the other.

**PATTERN** — fifth architectural axis: **exploration ↔ exploitation**. Different
in kind from the first four — it describes how a *search* is run, not where an
*architecture* sits.

**FLAGGED** — the six-word **competing conventions problem** (p11, as a con of
neuroevolution) is the *reason* for the *"recombination can be destructive"*
warning given four pages earlier under a different heading. Joining them is the
most useful cross-reference in the lecture — and together with *"without
recombination, evolution becomes a parallel gradient search"* they amount to a
serious charge against neuroevolution, assembled entirely from the lecture's own
statements.

**FLAGGED** — mutation is called *"random and unbiased"* and then given four
deliberate engineering biases (add connections at zero weight; select nodes using
the current connection scheme; delete in inverse proportion to weight;
`Prob(delete) > Prob(add)`). The recommendations are right; the characterisation
is wrong. Recorded as a sixth correspondence failure mode: **biologically named,
engineered anyway** — with uniform crossover (no chromosomal counterpart) and
universal haploidy as further instances.

**FLAGGED** — *"Better at food gathering = better at surviving = make more
offspring"* is written as a chain of equalities, which is circular as biology. It
is **not** circular as an algorithm, because fitness is stipulated in advance —
and that is the substantive difference between the metaphor and the method.

**FLAGGED** — `f(z) = 1/x²` mixes genotype and phenotype arguments, diverges at
`x = 0`, and does not state whether to maximise or minimise.

**VERIFIED** — `Pr(i) = f_i / Σ_j^μ f_j`; `P_LR(i) = (2−s)/μ + 2i(s−1)/(μ(μ−1))`
with `s ∈ [1,2]`; `p_c ∈ [0.5, 1.0]`; the 10/40/50 elite/mutated/recombined
default; `Φ = V(1−√ΔV)(1−i)` with `V`, `ΔV`, `i` all glossed.

**GAP** — reinforcement learning, at its widest yet. L11 supplies the
exploration–exploitation axis, a scalar quality signal instead of labels, and
robot control learned from a performance measure. Every ingredient of RL is now
present across L03, L08, L09, L10 and L11, and the algorithm has never been
written down.

**GAP** — no attribution. Four EA dialects, four originators, none named
(consistent with `adam-kendon`, `rodney-brooks`, `valentino-braitenberg`). The
roulette wheel's variance defect is named without its standard fix; *"generative
encoding, e.g. grammar"* is the most powerful idea on the page and gets three
words.

**GAP** — still no evaluation numbers, in eleven lectures.

**Wiki state:** 197 pages, 0 dead links, 0 orphans.

## [2026-09-21] ingest | Lecture 12 — Neuro-Symbolic and Explainable AI

Source: `raw/lectures/BioinspiredAIWdh12.pdf` (9 pages).

**Created (14):** [[L12-neuro-symbolic-and-explainable-ai]]; concepts
[[symbolic-ai]], [[neural-symbolic-integration]],
[[hybrid-integration-architectures]], [[knowledge-extraction]],
[[explainable-ai]], [[levels-of-abstraction]]; systems
[[weight-based-transfer]], [[hinton-diagram]], [[transducer-network]],
[[automata-extraction]], [[preference-moore-machine]],
[[class-activation-map]], [[layer-wise-relevance-propagation]].

**Updated (9):** [[gpt]], [[local-vs-distributed-representation]],
[[ann-brain-correspondence]], [[convolutional-network]],
[[hybrid-architecture]], [[learning-paradigms]],
[[simple-recurrent-network]], [[L06-hierarchical-vision]],
[[top-down-modulation]].

- **CONTRADICTION (major).** L02 presents distributed representation as an
  **advantage** (robustness, graceful degradation); L12 presents it as a **cost**
  ("difficult to understand and modify"). Same property, opposite sign, neither
  lecture citing the other. Flagged on both pages — this is the clearest case
  in the module of one trade-off taught twice as two one-sided claims.
- **CONTRADICTION.** L12's three-level stack (symbolic / neural / sensory) is
  drawn feed-forward only, contradicting L07 and L09's top-down modulation *and*
  its own "tight coupling (bidirectional)" bullet on the same page.
- **FLAGGED.** "The brain as a hybrid system supporting signals, symbols,
  structures, knowledge" — asserted with zero evidence and carrying the entire
  biological case for the field. Recorded as a **sixth failed correspondence
  standard: assertion by vocabulary**.
- **FLAGGED.** Three incompatible senses of "explanation" in one lecture
  (extraction from weights / attribution over the forward pass / LLM prose).
  ChatGPT's listed strengths are all about the medium, its weaknesses all about
  the content.
- **PATTERN.** Combine-or-arbitrate fork, 4th instance — and the first joining
  two things of *different representational kinds*.
- **PATTERN.** Symbol extraction from an ANN and single-unit recording from a
  cortex are the same activity, performed for the same reason. The module has
  shown both many times and never connects them.
- **RESOLVED (partial).** [[simple-recurrent-network]]'s context layer, an
  engineering trick in L03, is retroactively the *state register* of a finite
  state machine.
- **GAP.** Swarm and collective intelligence: **still absent through L12.**
- **GAP.** Reinforcement learning: 13th named-only appearance, now using an
  undefined internal term ("critic maps").
- **GAP.** No evaluation numbers anywhere in the module, still.

## [2026-09-21] ingest | Lecture 13 — Continual Learning (final lecture)

Source: `raw/lectures/BioinspiredAIWdh13.pdf` (8 pages, 18.02.2024).
**All 13 lectures now ingested.**

**Created (16):** [[L13-continual-learning]]; concepts [[continual-learning]],
[[catastrophic-forgetting]], [[stability-plasticity-dilemma]],
[[continual-learning-strategies]], [[continual-learning-metrics]],
[[memory-replay]], [[complementary-learning-systems]],
[[continual-language-learning]], [[developmental-and-curriculum-learning]],
[[intrinsic-motivation]]; systems [[growing-dual-memory]], [[associative-gwr]],
[[bert]], [[elastic-weight-consolidation]], [[progressive-neural-network]],
[[carl]].

**Updated (11):** [[gwr-network]] (major — full algorithm),
[[gamma-gwr]], [[self-organising-map]], [[learning-paradigms]],
[[transfer-learning]], [[local-vs-distributed-representation]],
[[ann-brain-correspondence]], [[hebbian-learning]], [[synaptic-plasticity]],
[[gpt]], [[multisensory-integration]].

- **RESOLVED (major).** ~~No evaluation measure anywhere in the module.~~ L13
  gives **average accuracy** `A_k` and **forgetting** `f_j^k`. Still no reported
  numbers, but there is now a definition of what a result would be. The
  forgetting metric uses the model's own best past performance as the reference,
  which is the correct control given positive backward transfer.
- **RESOLVED (major).** ~~GWR named and used in L10 but never specified.~~ L13 p5
  gives all fifteen steps: growth criterion, both update rules, habituation,
  edge ageing, pruning. Transcribed in full with pseudocode into
  [[gwr-network]]. Key undrawn insight: **growth requires poor fit AND a
  habituated winner** — the conjunction is what stops a neuron per sample.
- **RESOLVED.** ~~L13's role for the SOM/GWR family.~~ They were taught because
  they are the module's only continual learners; L13 says so outright.
- **SYNTHESIS.** The distributed-representation trade-off completed across three
  lectures: L02 advantage (robustness), L12 cost (legibility), L13 cost
  (retention). One root — **a concept has no address** — recorded on
  [[local-vs-distributed-representation]].
- **SYNTHESIS.** **Negative backward transfer *is* catastrophic forgetting.** L13
  teaches both, six pages apart, without connecting them.
- **VERIFIED.** "Disrupting slow wave sleep impairs long-term memory
  consolidation" is the **first and only falsifiable experimental result in the
  whole module**. Recorded as a **seventh correspondence standard: convergence
  from both directions** — replay was invented for optimiser reasons and the
  biology matched afterwards, which is stronger evidence than inspiration.
- **FLAGGED.** "Catastrophic forgetting affects all connectionist models" is
  contradicted by the lecture's own next page ([[progressive-neural-network]]).
- **FLAGGED.** Rehearsal ("explicitly stored training samples") violates
  continual learning's own stated requirement of no re-access to old data.
- **FLAGGED.** [[growing-dual-memory|GDM]]'s diagram is feed-forward only, but
  replay is downward by definition — same drawing problem as L12.
- **GAP (permanent).** **Reinforcement learning is never defined.** Fourteenth
  appearance, in a fully drawn actor–critic diagram
  ([[intrinsic-motivation]]). Architecture, vocabulary, signal type and central
  trade-off all taught; the algorithm never written down.
- **GAP (permanent).** **Swarm and collective intelligence never appear.** Absent
  from all 13 lectures.
- **GAP (permanent).** **"Transformer" is never defined**, despite four systems
  across four lectures being described as transformer-based.
- **NOTE.** The module's closing self-assessment is the modest one: "current
  models: far from providing flexibility, robustness & scalability".
