---
type: concept
title: Hybrid Architecture
sources: [L05, L07, L08, L09, L10, L12]
tags: [design, methodology, robotics]
---

# Hybrid Architecture

Combining a hand-written algorithm with a learned component, each doing what it
is better at.

L05 is the first lecture in the module to argue for **not** learning something.

## The argument (L05, p4)

**Why hybrid localisation and tracking?**

- **Algorithmic sound source localisation is well understood algorithmically.**
- **Cross-correlation does not require training to provide azimuth angle.**
- **Neural predicting of source enables a quicker response and can learn
  temporal sequences.**

## The decision rule

Read those three bullets as a criterion:

| The problem is… | Use… | Because |
|---|---|---|
| fully determined by known physics | an algorithm | there is nothing to fit; training would only approximate a formula you already have |
| uncertain, idiosyncratic, or time-varying | a learned model | no closed form exists |

In [[hybrid-acoustic-tracking]] the split lands as:

- **Where is the source *now*?** → geometry. `a = c·t_ITD`, `Θ = arccos(a/b)`.
  Exact, instant, zero training data.
- **Where will it be *next*?** → learned. Depends on how *this* source happens
  to move — the lecture's phrase is *learning and adaptation to acceleration and
  deceleration*.

## Why this is a notable stance for the module

L03 and L04 reach for learning by default. Every problem becomes a loss function
and a gradient. L05 pushes back: a neural network that learns `arccos` from
examples is strictly worse than `arccos` — slower to build, less accurate, and
liable to fail outside its training distribution.

The reframing is that **learning capacity is a resource to spend where the
uncertainty actually is.** Spending it on solved physics is waste.

## The second hybrid

The same seam appears one level down in
[[hybrid-spiking-localisation-network]], but for a different reason:

```
  spiking front end (MSO/LSO/IC)  │  feed-forward classifier
  hand-wired from anatomy         │  learned
```

Here the split is not *understood vs uncertain* but *trainable vs not*: spiking
networks are difficult to train with [[backpropagation]], so the biologically
specified part is fixed and only the read-out is fitted.

Two different justifications, same architecture. The lecture does not
distinguish them.

## The tension it creates

A hybrid system is **not** bio-inspired in L01's strict sense. The criterion in
[[intelligent-behaviour]] requires the representation *and* the processing to
follow the biological principle; `arccos` follows trigonometry.

The defence is that the algorithm and the biology compute the same function —
[[cross-correlation-localisation]] and the [[jeffress-model]] are the same
operation in different substrates. If so, the hybrid is bio-inspired at the
level of *what is computed* while being conventional at the level of *how*.
Whether that counts is exactly the question L01 left open, and L05 does not
raise it.

## See also

[[hybrid-acoustic-tracking]] · [[hybrid-spiking-localisation-network]] ·
[[cross-correlation-localisation]] · [[simple-recurrent-network]] ·
[[intelligent-behaviour]] · [[levels-of-abstraction]] ·
[[ann-brain-correspondence]] · [[L05-robot-sound-localisation]]


## L07 adds a second axis: speed and authority

L05 argued that a system should be heterogeneous ? some parts fitted from data,
some written down. [[L07-crossmodal-processing]]'s
[[cortico-collicular-architecture]] is heterogeneous along a different axis:

| | Subcortical ([[superior-colliculus]]) | Cortical |
|---|---|---|
| Speed | fast, reflexive | slower |
| Knowledge | local, immediate | context, congruence |
| Role | does the integration | **biases** it |

The coupling is the interesting part. Cortex does not override the SC and does
not sit in its path; it sends a **modulatory** projection that shifts how the
subcortical fusion is done. See [[top-down-modulation]].

So the L05 question *"which parts should be learned?"* gains a companion:
**"which parts should be fast, and which should be allowed to overrule them ?
and by how much?"** A modulatory rather than a driving connection is a way of
saying *advise, do not command*, and the module has no other example of it.


## L08: a third axis ? model or no model

L05 asked which parts should be **learned**; L07 asked which should be **fast**
and which should **advise**. [[L08-behaviour-based-robotics]] asks whether there
should be a **world model** at all.

| Axis | Lecture | Poles |
|---|---|---|
| learned ? specified | L05 | neural net ? hand-written cross-correlation |
| fast ? contextual | L07 | [[superior-colliculus]] ? cortex |
| **modelless ? modelled** | **L08** | [[reactive-agent]] ? [[functional-decomposition]] |

And L08 does not settle its own axis: it rejects the pipeline on page 1 and
rebuilds it as [[imitation-learning]] on page 9, then restates the whole fork as
[[object-picking-architecture|modular vs end-to-end]] for learned systems.

> [!note] Three axes, no composite
> A real robot must take a position on all three at once, and the module has
> never drawn the cube. The nearest thing to a system that answers all three is
> [[nico]], which is described as a platform rather than as an architecture.


## L09: attention as an architectural component

The module's fourth axis. [[attention]] is not a module in the pipeline but a
**control signal over it** — the [[auditory-attention-model|ASA]] version biases
*every* stage.

| Axis | Lecture | Question |
|---|---|---|
| learned ↔ specified | L05 | which parts are fitted? |
| fast ↔ contextual | L07 | which parts advise? |
| modelless ↔ modelled | L08 | is there a world model? |
| **stimulus-driven ↔ goal-driven** | **L09** | who decides what gets processed? |

The fourth turns out to be a generalisation of the second: L07's cortical
modulation of [[superior-colliculus|SC]] fusion **is** endogenous attention, and
L08's goal intention encoding is too. L09 supplies the name the two earlier
lectures were missing.


## L10 ? two more hybrids, and a new reason to build one

[[L10-gesture-recognition]] contributes two, and the second supplies the best
justification the module has given for hybridisation.

**[[cnn-lstm]]** ? a **modality-internal** hybrid. The CNN supplies invariance in
space, the LSTM invariance in time; neither can supply the other's. The
components are chosen for the **invariance** each provides.

**[[snapshot-model]]** ? a **representation** hybrid. One channel sees motion
(CNN-LSTM over differential images), the other sees posture (snapshots at motion
peaks). The stated reason is that the channels have **complementary failure
modes**: subtle gestures defeat the motion channel and are legible to the
posture channel.

> [!note] The strongest argument for hybrids yet
> Earlier hybrids in the module are justified by *this component models that
> brain area* or *this works better*. The snapshot model is justified by an
> explicit account of **where each component fails** ? and its stated limitation
> follows from that account rather than from experiment: *"gestures with similar
> motion AND similar hand pose"*, i.e. exactly when both channels are blind.
>
> That is how an architecture argument should read, and it is not flagged as
> exemplary in the source.

Both fuse by **concatenation at a classifier** ? [[fusion-strategies|late
fusion]], fixed weights, no reliability estimate. A
[[gated-multimodal-unit|learned gate]] would let the snapshot model lean on
posture when motion is weak, which is [[inverse-effectiveness]] for gestures.
The source does not raise it.

[[gamma-gwr|Gamma-GWR]]'s `G^P` / `G^M` ? `G^I` exercise-prediction system is the
same posture/motion split again, one page later, learned **unsupervised** ? and
its `G^I` is the module's first *learned, unsupervised* join.

## L12 ? a new axis of hybridity

Every hybrid in the module up to here combined systems that process the **same
kind** of thing differently: [[two-visual-streams]],
fast and contextual channels (L07), [[subsumption-architecture|layered
behaviours]] (L08). L12's [[neural-symbolic-integration|neuro-symbolic]] hybrid
is the first that joins systems whose **representations are of different kinds** ?
distributed vectors and discrete symbols ? and it is therefore the first where
the interface itself is the hard problem.

Its taxonomy is also the module's most explicit statement of *how* to combine
anything:

| Degree | Modules separable? | Communication |
|---|---|---|
| **Loose coupling** | yes | unidirectional |
| **Tight coupling** | yes | bidirectional |
| **Full integration** | no ? fully embedded | they "work together" |

That ladder is a general vocabulary and retroactively classifies the module's
earlier hybrids: subsumption is loose coupling, [[top-down-modulation|top-down
modulation]] makes a system tightly coupled, and
[[gated-recurrent-network|gating inside a cell]] is full integration.
[[neural-symbolic-integration]] is the only lecture to offer these words.
