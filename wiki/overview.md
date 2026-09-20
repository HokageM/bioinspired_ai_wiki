---
title: Overview
type: overview
tags: [foundations]
sources: [L01, L02, L03, L04, L05]
created: 2026-09-20
updated: 2026-09-21
status: developing
---

# Bio-Inspired AI — Module Overview

A provisional map of the module, built from the lectures ingested so far.
**Ingested: L01–L05 of 13.** Expect this page to be rewritten as the arc
becomes clear.

## The thesis

> **Bio-inspired AI allows to solve problems informed by the best problem
> solvers in nature.** (L01)

The module's recurring move: identify a mechanism nature uses to solve a hard
problem, extract its principle, and reimplement it. What counts as
"bio-inspired" is set out in [[intelligent-behaviour]] — it is not enough to be
motivated by biology; the *representation* and *processing* must follow the
biological principle.

## The arc so far

| Lecture | Topic | Level of abstraction |
|---|---|---|
| [[L01-introduction-to-bio-inspired-ai]] | Framing, requirements, navigation example | Systems / behaviour |
| [[L02-spiking-neural-networks]] | Neurons, coding, dynamics, plasticity | Single neuron / synapse |
| [[L03-computational-neural-networks]] | Abstraction to nodes; supervised training; sequences | Network / algorithm |
| [[L04-embodied-language-processing]] | Language in the brain; grounding; maps, robots and transformers | Brain areas / whole agent |
| [[L05-robot-sound-localisation]] | ITD/ILD, Jeffress, cross-correlation, a working robot | Brainstem nucleus / full system |
| [[L06-hierarchical-vision]] | Retina to V1 to CNN; weight sharing and its repairs | Sensory hierarchy / architecture |
| L07–L13 | *not yet ingested* | — |
| L08 | Behaviour-based robotics | Against functional decomposition; Braitenberg, subsumption, motor schemas, NICO | [[L08-behaviour-based-robotics]] |
| L09 | Bio-inspired attention | Selective/sustained, exo/endo, three networks, saliency, ASA, social attention | [[L09-bio-inspired-attention]] |
| L10 | Neurally-inspired gesture recognition | Gesture continuum, MCCNN, CNN-LSTM, snapshot model, GWR and Gamma-GWR, OpenPose, CLIP | [[L10-gesture-recognition]] |
| L11 | **Evolutionary computing** | Four pillars, genotype/phenotype, the EA scheme, all operators, neuroevolution, evolved navigation | [[L11-evolutionary-computing]] |
| L12 | **Neuro-symbolic & explainable AI** | The two traditions compared, four ways to hybridise, rule and automaton extraction, CAM and LRP | [[L12-neuro-symbolic-and-explainable-ai]] |
| L13 | **Continual learning** | Catastrophic forgetting, stability–plasticity, four strategies, GWR in full, GDM, CLL, evaluation metrics | [[L13-continual-learning]] |

## The two directions

The most useful thing to hold after the first three lectures is that **L02 and L03 travel
the same road in opposite directions**, and end up with incompatible learning
rules.

| | L02 | L03 |
|---|---|---|
| Direction | Biology → model, keeping fidelity | Biology → model, discarding for tractability |
| Method | Add mechanism until it spikes | Strip mechanism until it trains ([[levels-of-abstraction]]) |
| Representation | Spike times ([[temporal-coding]]) | Activation levels ([[rate-coding]]) |
| Learning rule | [[hebbian-learning]], [[stdp]] | [[perceptron-learning-rule]], [[backpropagation]] |
| Information used | **Local** — the two ends of one synapse | **Global** — errors from the whole downstream network |
| Supervision | Unsupervised | Supervised |

L03 asserts these are the same thing (*backpropagation ↔ plasticity*, see
[[ann-brain-correspondence]]) — a claim the wiki flags as the weakest in the
module so far.

## The organising fork

The single most useful structure to hold from L02 is the split in
[[neural-coding]] — one diagram that explains the shape of two research fields:

```
                 modelling neuron communication
                    /                      \
   coding by ACTIVITY LEVEL          coding by FREQUENCY / TIMING
            |                                    |
   [[mcculloch-pitts-neuron]]            spiking neurons
            |                                    |
      Connectionism                    Computational Neuroscience
      ("rate-coded")                   ([[spiking-neural-network]])
```

Left branch: [[rate-coding]]. Right branch: [[temporal-coding]].

## The ladder in L02

L02 climbs from static to spiking in five steps, each one fixing a limitation of
the last. This is the spine of the lecture:

1. **[[mcculloch-pitts-neuron]]** — weighted sum, threshold,
   [[activation-function]]. *Limitation: static — output determined by current
   input.*
2. **[[discrete-dynamic-neuron]]** — add a recurrent weight $\mu_i$ and a delay.
   *Gains memory. Limitation: discrete time.*
3. **[[continuous-dynamic-neuron]]** — the ODE form, $\mu \leftrightarrow 1/\tau$;
   an RC circuit; a simplification of [[hodgkin-huxley-model]]. *Limitation: one
   time constant for all inputs.*
4. **[[synaptic-kernel]]** — each synapse gets its own temporal characteristic.
5. **[[integrate-and-fire]]** / **[[spike-response-model]]** — add threshold,
   reset and refractory feedback. *Now spiking.*

Learning runs alongside: [[synaptic-plasticity]] → [[hebbian-learning]] →
[[stdp]], where each step adds what the previous lacked.

## The ladder in L03

L03 has its own ladder, each rung again fixing the previous one's limitation:

1. **[[mcculloch-pitts-neuron]]** + **[[perceptron-learning-rule]]** — trainable,
   with a convergence guarantee. *Limitation: one hyperplane —
   [[xor-problem]].*
2. **[[multi-layer-perceptron]]** — stack them. *Limitation: no rule can train
   the hidden layer.*
3. **[[backpropagation]]** — propagate error derivatives backwards. *Limitation:
   overfits, and needs practical scheduling.*
4. **[[overfitting-and-underfitting]]** + [[regularisation]], [[dropout]],
   [[batch-vs-online-training]]. *Limitation: fixed-width input, no sequences.*
5. **[[recurrent-neural-network]]** → [[simple-recurrent-network]].
   *Limitation: [[vanishing-gradient-problem]].*
6. **[[gated-recurrent-network]]** — additive cell state.

## What L04 changes

L04 is the module's first jump up a level: from *how does a unit compute* to
*how does a whole agent mean anything*. It is also the first lecture where the
module's own criterion from [[intelligent-behaviour]] starts to bite.

Its argument has a clean shape ? a historical narrowing followed by an
engineering widening:

```
phrenology  →  Broca/Wernicke  →  dual stream  →  embodied, whole-brain
(guessing)     (localisation)     (pathways)      ([[embodied-language-representation]])
```

and then five models of that last box, which do not agree with each other:

| Model | Learning | Local? | Embodied? |
|---|---|---|---|
| [[multi-layer-associator]] | Hebbian | yes | no |
| [[self-organising-map]] | competitive | yes | no |
| [[imitation-network]] | supervised gradient | no | **yes** |
| [[word2vec]] | self-supervised gradient | no | no |
| [[gpt]] | self-supervised gradient | no | no |

**No model is both local and embodied**, and the two that work best on real
language are neither. The lecture does not comment. This is the sharpest form
yet of the tension on [[ann-brain-correspondence]].

Two standing gaps do close here. Unsupervised learning finally gets an algorithm
([[self-organising-map]]), and the L03 sequence ladder finally gets its ending
— [[gpt]]'s *attention instead of recurrence* removes the
[[vanishing-gradient-problem]] by removing recurrence.

And a fourth paradigm arrives: [[self-supervised-learning]], which L03's
three-way split had no room for.

## What L05 changes

L05 is the module's first **complete working system**, and the lecture that
settles two arguments the earlier ones left open.

**It pays L02's debt.** Spiking neurons were built in detail in L02 and then
ignored for two lectures of rate-coded networks. Sound localisation is the task
that needs them: [[interaural-time-difference]] is a gap of tens of microseconds
between the ears, and no firing rate resolves that. [[temporal-coding]] stops
being one branch of a fork and becomes a requirement.

**It supplies a correspondence that actually holds.** The
[[cross-correlation-localisation]] algorithm and the [[jeffress-model]]'s
delay-line array compute the *same function* ? the algorithm iterates over
delays, the brainstem instantiates them in parallel. Both sides can be written
down and compared, which is precisely what *backpropagation ? plasticity* never
allowed. See [[ann-brain-correspondence]].

**And it introduces a new stance: [[hybrid-architecture]].** L03 and L04 reach
for learning by default. L05 argues for *not* learning what is already
understood ? use `arccos` for the geometry, spend the learning on the source's
future trajectory, which has no closed form.

| System | Stage 1 | Stage 2 | Stage 3 |
|---|---|---|---|
| [[hybrid-acoustic-tracking]] | algorithmic correlator | [[simple-recurrent-network]] | motor control |
| [[hybrid-spiking-localisation-network]] | spiking MSO/LSO/IC | feed-forward classifier | motor control |

## What L06 changes

L06 is the lecture where the module's central claim stops being a slogan and
starts being an argument.

**1. It supplies a correspondence with a paper trail.** Every previous
brain↔network claim was asserted. This one is *derived*, and the vocabulary
proves it: [[david-hubel]] and [[torsten-wiesel]] measured **simple** and
**complex** cells; [[kunihiko-fukushima]] built **S-cells** and **C-cells**;
[[yann-lecun]] made the result trainable. The `S` and `C` stand for the words
Hubel and Wiesel used. See [[simple-complex-hypercomplex-cells]],
[[neocognitron]], [[convolutional-network]].

**2. It then attacks its own model.**

> **CNNs require weight sharing, which real neurons cannot do.**

This is the same objection the wiki has been making against *backpropagation ↔
plasticity* since L03 — an algorithm needing information at a synapse that could
not physically be there — except that here **the lecture makes it itself**, and
then does the engineering. [[data-augmentation]] is tried and honestly reported
as *only small performance improvement*. [[dynamic-weight-sharing]] is tried and
works on paper: a local rule, computable at the synapse, that provably descends
the global objective `Σ(z_i − z_j)²` whose minimum *is* weight sharing.

That is the first time in the module that a plausibility claim has been
**stated as a constraint, tested, and reported honestly**. See
[[ann-brain-correspondence]].

**3. It corrects an earlier lecture.** L06's *dorsal = where, ventral = what*
contradicts L04's labelling of comprehension as dorsal. The wiki flagged this at
L04 ingest against external reference; L06 confirms it from inside the module.
See [[dual-stream-hypothesis]].

**4. It finally gives Hebb a decay term.** `−γ(w_i − w_i^init)` answers the
self-amplification problem raised in orange on L02 p7 and left open through L04.
Five lectures. The notes never connect the two. See [[hebbian-learning]].

**5. And it widens "Hebbian" a third time.** The rule that does all this is
anti-Hebbian in sign. Together with L04's [[self-organising-map]] and
[[multi-layer-associator]], the pattern is now firm: *the module uses "Hebbian"
to mean local, not correlational.*

### A new organising idea: layers that alternate in kind

L02 classified networks by **connectivity**. L06 classifies them by **what each
layer is for**:

```
  detect  →  discard position  →  detect  →  discard position  →  classify
   conv         pooling            conv        pooling            dense
   S-cell       C-cell             S-cell      C-cell
   simple       complex            simple      complex
```

Invariance is *bought* by deliberately destroying information, and at every
level the same trade appears: the retina discards absolute brightness to gain
contrast; pooling discards position to gain translation tolerance. See
[[pooling]], [[local-vs-distributed-representation]].



## What L07 changes

L06 asked how a hierarchy is built inside **one** sense. L07 asks the question
the module had been avoiding: **what happens when two senses disagree?**

**1. A validation standard that could actually fail.** Earlier lectures argued
for brain-likeness by asserting a mapping (L03) or by writing both sides down and
comparing them (L05, L06). L07's [[histogram-based-som]] is judged by whether it
**reproduces measured biological signatures** — depression, the
[[spatial-principle]], enhancement, [[inverse-effectiveness]]. That is the
strongest standard the module has used, because a plausible model could fail it.
See [[ann-brain-correspondence]].

**2. Uncertainty becomes a first-class quantity.** Every representation before
this encoded *what* is there. [[optimal-cue-integration]] needs *how sure we
are*, and so the SOM is rebuilt to store distributions rather than prototypes.
The output is a **population-coded probability density**, and its width is the
confidence. See [[local-vs-distributed-representation]].

**3. The same gate, twice.** The [[gated-multimodal-unit]]'s
`σ · tanh(·) + (1−σ) · tanh(·)` is the L03 GRU update gate with modalities in
place of time. The module has now used *learn how much to trust each of two
sources* twice, without noticing.

**4. Advising rather than commanding.** The
[[cortico-collicular-architecture]] has cortex **modulate** subcortical fusion
instead of overriding it — the module's only non-driving connection. See
[[top-down-modulation]].

**5. What it leaves undone.** Two explanations of the
[[ventriloquism-effect]] are set out and never adjudicated, though one is the
limiting case of the other. [[inverse-effectiveness]] is never connected to
[[optimal-cue-integration]] despite being the same claim. And L07 is the only
lecture in the module with **no people, no dates and no citations at all**.


## What L08 changes

The first lecture about a **whole agent acting in a world**, and the first whose
central claim is **negative**: the obvious way to build a robot is wrong.

**1. Behaviour stops being evidence.** A [[braitenberg-vehicle]] with four wires
is *goal-directed, fast, flexible, adaptive* and has no representation of
anything. If **complex behaviour may simply be the reflection of a complex
environment**, then observed sophistication is a fact about the agent?world
system, not the agent. This is the sharpest warning in the module against
arguing from resemblance — and it lands one lecture after L07's strongest
argument *from* resemblance. See [[intelligent-behaviour]].

**2. A third architectural axis.** L05 asked what should be **learned**; L07 what
should be **fast**; L08 asks whether there should be a **world model** at all.
See [[hybrid-architecture]].

**3. The combine-or-arbitrate fork, again.** [[behaviour-coordination]] is
L07's [[fusion-strategies]] on the output side: sum the vectors
([[motor-schema]]) or take the winner ([[subsumption-architecture]]). Summing
is smooth and can produce an action nobody wanted — which is exactly the
[[local-minima-problem]].

**4. Self-supervision with a body.** [[neural-grasp-learning]] makes the robot
generate its own labels by **placing** an object and then regrasping it. The
supervisory signal comes from acting, which is what situatedness was supposed to
buy. Its limit is that the data can only cover what the robot could already do.

**5. The lecture contradicts itself, usefully.** It rejects
[[functional-decomposition]] on page 1 and rebuilds it as
[[imitation-learning]] on page 9. The honest reading is that the fork is not
resolvable by slogan: reactive wins at *stay alive*, symbolic wins at
*reproduce what I showed you once*.

**6. What is still missing.** Reinforcement learning — in the one lecture with
an agent, actions and outcomes, whose own disadvantage list names the
credit-assignment problem outright.


## What L09 changes

The lecture that **pays two old debts** and incurs one new one.

**1. Attention, defined.** Open since L04, when [[gpt]] was explained as
*attention instead of recurrence* and nothing more. L09 gives the full
construct — selection under **limited capacity**,
[[exogenous-and-endogenous-attention|exogenous versus endogenous]], the three
[[attention-networks]], [[saliency-map|saliency]], [[winner-take-all]].

**2. And the word turns out to be a pun.** Transformer self-attention is not
capacity-limited, is not serial, and **keeps the losers**. The one real point of
contact is that a query against keys is a top-down bias over candidates. The
module never puts the two side by side; the wiki does, on [[attention]]. This is
a third failure mode for [[ann-brain-correspondence]]: not a weak correspondence
but **a terminological coincidence mistaken for one**.

**3. The cocktail party, answered.** Open since L05.
[[auditory-scene-analysis]] supplies the shape — group, segregate, compete —
and the key addition: **localisation is not separation**. Knowing where each
talker is does not tell you which energy is theirs.

**4. One primitive, named at last.** [[winner-take-all]] is the SOM's
best-matching unit, L07's max fusion, L08's action selection and subsumption, and
L09's saliency read-out. Five lectures, five names. L09 also gives the reason it
recurs: **lateral inhibition implements argmax locally**, with no comparator.

**5. Method arrives.** [[reaction-time]] and the
[[additive-factors-method]] are the module's first tools for inferring internal
structure from external measurement, and
[[human-robot-collaboration]] runs its only complete loop — observe humans,
model, implement, re-test. Neither tool is actually used to test a model in L09.

**6. Top-down control, escalated.** The goal now biases **every** stage of a
pipeline, including grouping — so what counts as *one sound* depends on what
you are listening for. See [[top-down-modulation]].

## What L10 changes

**A network that changes its own architecture.** Every learning rule in nine
lectures — [[hebbian-learning]], [[perceptron-learning-rule]],
[[backpropagation]], the [[self-organising-map|SOM]] update — adjusts the
*strength* of connections laid down by a human choosing a layer width.
[[gwr-network|GWR]] chooses the size itself: grow a node when the input is poorly
covered **and** the winner is already well-trained. The second condition is the
good idea — novelty alone is not a reason to grow, only *persistent* novelty is.
This is the module's first structural plasticity, and it is not flagged as a
departure.

**Recurrence without a gradient.** [[gamma-gwr|Gamma-GWR]] gives each node a
**context vector** and puts it in the distance function, so the same input
arriving from different histories selects different winners. Sequences, learned
competitively, unsupervised. Set beside [[gated-recurrent-network|LSTM]] it makes
the point that recurrence is a *representational* trick, not a gradient trick.

**The module's only negative result.** *"3D kernel: no significant advantage over
2D kernels."* Nine lectures of architectures that work, and here one that does
not — with a structural reason available: a 3D kernel has a fixed temporal
extent, so it is time-*aware*, not time-*invariant*. That is why
[[cnn-lstm]] exists, and the derivation from a stated failure is the cleanest
piece of architectural reasoning in the module.

**A critique that generates the lecture.** [[deep-network-tradeoffs]] lists three
objections and each one is answered by the next system: *not appropriate for
time* → CNN-LSTM; *data-hungry* → [[openpose]] skeletal input; *specialist, not
adaptive* → GWR. Whether the second is really an answer is doubtful — OpenPose
is itself a large supervised network, so the data cost is moved, not removed.

**The stroke is where the meaning is.** [[gesture-phases]] → the informative
moment is the stroke → find it as a peak in the
[[motion-intensity-profile]] → classify that frame statically
([[snapshot-model]]). A behavioural prior doing an engineer's work — and a
fourth kind of bio-inspiration, distinct from the three on
[[ann-brain-correspondence]]'s scorecard: not *the mechanism resembles a brain*
but **the signal has a known structure, so assume it**.

**The honest hybrid.** [[snapshot-model]] justifies its two channels by their
**complementary failure modes**, and its stated limitation follows from that
account rather than from experiment: it fails when motion *and* pose are both
uninformative. No other architecture in the module argues this way.

**Grounding, third position.** [[contrastive-language-image-pretraining|CLIP]]
sits between [[word2vec]] (text only, ungrounded) and
[[embodied-language-representation]] (grounded in action): meaning constrained by
**paired perception**, with no body and no consequences. It is the strongest test
case for L04's open question and arrives without reference to it.

**And the hole gets harder to ignore.** L10 names the **striatum** and
**action selection** — the anatomy of reinforcement learning and the function it
performs — and still writes down no reward, no value function, no policy.
[[reservoir-computing]] is named once, in the summary, and never explained. Four
lectures have now reached action selection from four directions
([[learning-paradigms]], [[behaviour-coordination]], [[winner-take-all]],
[[reservoir-computing]]) without converging.


## What L11 changes

**A second tradition.** Ten lectures of neural inspiration, then one of
evolutionary inspiration, sharing almost no vocabulary with them. The wiki's
standing complaint — that a module called *Bio-Inspired AI* contained only one
kind of biological inspiration — is answered here.

**Bio-inspiration one level up.** *"Powerful problem solvers in nature → Brain:
'wheel' → evolutionary mechanism, that created the human brain."* The brain is
the **output** of a search process, so copy the process instead of the product.
It is the module's most defensible correspondence claim, because the
[[four-pillars-of-evolution|four pillars]] are substrate-neutral: running them on
candidate solutions is not a metaphor for evolution, it is evolution on a
different substrate. Recorded as a fifth standard on
[[ann-brain-correspondence]] — and a sixth **failure** mode alongside it, since
the operators drift from the biology whenever engineering demands it.

**The hyperparameter question, asked at last.** *"How to select necessary
topology, weights and efficient learning parameters?"* Ten lectures chose layer
counts, unit counts and learning rates silently. L11 names the omission and
proposes to **search** for the answers — see [[neuroevolution]].

**Optimisation without gradients.** An [[evolutionary-algorithm]] needs only that
solutions can be **ordered**. No derivative, no differentiability, no metric
space. That is why it can optimise a **topology**, a discrete object no gradient
reaches, and why it is the first method in the module that could not be trained
by [[backpropagation]] even in principle.

**A population instead of a point.** Every optimisation through L10 refines one
parameter vector. An EA keeps many and lets them compete, which is what buys
escape from local optima — [[fitness-landscape]] is
[[local-minima-problem|L08's problem]] in high dimensions, and
[[recombination]] is the module's **only non-local search move**. That it is
*"often a destructive jump"* is the same property seen from the other side.

**The competing-conventions argument, assembled but not made.** The lecture
states that recombining networks is destructive (p7), that
[[competing-conventions-problem|`n!` genomes encode each function]] (p11, six
words), and that *"without recombination, evolution becomes a parallel gradient
search"* (p8). Together those three sentences are a serious charge against
neuroevolution with direct encodings — built entirely from the lecture's own
material, across three pages, under three different headings.

**Structure search, twice, by opposite means.** [[gwr-network|GWR]] (L10) adapts
capacity to the **data**, online and cheaply. [[neuroevolution]] (L11) adapts it
to the **task**, across generations and expensively. Consecutive lectures, same
question, no cross-reference — and they are complementary, not competing.

**L08's thesis becomes a method.** [[collision-free-navigation]]'s fitness
function mentions no goal, no map and no obstacle — only wheel speed, wheel
symmetry and sensor activation — and navigation emerges from maximising it. That
is [[embodiment-and-situatedness]] turned from a demonstration into an
engineering procedure, and the strongest support the module offers for
[[intelligent-behaviour|"intelligence is in the eye of the observer"]].

**And reinforcement learning is now conspicuous by its absence.** L11 supplies the
[[selection-pressure|exploration–exploitation]] axis, a scalar quality signal in
place of labels, and robot control learned from a performance measure. Every
ingredient of RL is present across five lectures; the algorithm has never been
written down.


## What L12 changes

L12 is the module turning round to look at itself. For eleven lectures the
answer to every question was *a network* — and L12 opens by naming the tradition
that said the answer was **rules**, and asking what each is for:

> **function approximation vs cognition**

This is the module's sharpest sentence, and the first acknowledgement that the
whole enterprise had a rival.

**The trade-off finally stated — with the sign flipped.** L02 taught
[[local-vs-distributed-representation|distributed representation]] as a virtue:
robust, generalising, no single point of failure. L12 teaches it as a cost:
*"difficult to understand and modify"*, compact **but** distributed. Both are
right, because they are the same fact. What makes a code survive damage — no unit
is individually meaningful — is what makes it unreadable. **You cannot lesion a
concept, and you cannot find one either.** The module never puts the two halves
together; the wiki now does.

**A general vocabulary for combining things, arriving eleven lectures late.**
[[hybrid-integration-architectures|Loose coupling / tight coupling / full
integration]] is the first precise language the module offers for *how* two
systems can be joined, and it retroactively classifies everything earlier:
[[subsumption-architecture|subsumption]] is loose, [[top-down-modulation]] makes a
system tight, [[gated-recurrent-network|gating]] is full integration. That it
appears in the penultimate lecture rather than the third is the single biggest
structural weakness of the module's ordering.

**Combine-or-arbitrate, fourth instance — and the first across kinds.** L07 fused
sensory channels, L08 arbitrated motor behaviours, L10 chose between posture and
motion. Each joined things of the *same type*. L12 joins a vector to a symbol,
which is why the interface, not the components, is the problem.

**Three different things called "explanation".** Rules pulled from weights
([[knowledge-extraction]], [[automata-extraction]]); attribution over the forward
pass ([[class-activation-map]], [[layer-wise-relevance-propagation]]); and an LLM
writing prose about a model it has never seen ([[gpt]]). Only the first two are
derived from the system being explained. The lecture lists all three as options.

**One real equation.** [[layer-wise-relevance-propagation|LRP]]'s
`R_j = Σ_k (z_jk / Σ_j z_jk) R_k` is the only XAI method here given
mathematically, and it is [[backpropagation]]'s machinery carrying a different
quantity: relevance instead of error, **conserved** rather than minimised. The
backward pass, it turns out, is a general device for attributing an output to its
causes.

**The correspondence scorecard gains a sixth, failed standard.** *"The brain as a
hybrid system supporting signals, symbols, structures, knowledge"* — no study, no
area, no measurement. **Assertion by vocabulary**: the architecture's four levels
projected onto the brain, then read back as justification for the architecture.
And the lecture's own second half undermines it: if the brain really is part
sub-symbolic, then it is exactly as opaque as an ANN, and extracting symbols
explains neither. [[ann-brain-correspondence]].

**Still missing:** swarm and collective intelligence, absent through twelve
lectures; reinforcement learning, named for the thirteenth time and now with an
undefined internal term ("critic maps"); and any number measuring anything.

## What L13 changes

The last lecture is the best one, and it is the only lecture that both **states a
problem precisely** and **admits the field has not solved it**.

**It names what the module had been doing all along.** Twelve lectures trained
models once, on fixed datasets, and asserted that they worked. L13 says that
setting is called *batch learning*, that it is not how anything alive learns, and
that [[catastrophic-forgetting|the moment you relax it everything breaks]]. In
that light the module's odd insistence on [[self-organising-map|SOMs]] and
[[gwr-network|GWR]] — unsupervised, incrementally growing, no output layer to
resize — stops looking like a detour. They were the only continual learners in
the syllabus, and only the final page says so.

**The module's oldest gap closes: there are finally metrics.** `A_k` for average
accuracy and `f_j^k` for forgetting ([[continual-learning-metrics]]). Thirteen
lectures asserted that systems worked; the last one defines what working would
mean. There are still no reported numbers anywhere, but the forgetting metric is
carefully built — measured against the model's own best past self, which is the
only reference that survives positive backward transfer.

**And GWR is finally specified.** Fifteen steps, both update rules, habituation,
edge ageing, pruning ([[gwr-network]]). The design insight is hidden in an `and`:
**growth requires a poor fit AND a habituated winner**, so a new neuron gets
time to settle before its failures count. Neither L10 nor L13's prose mentions
it.

**The distributed-representation trade-off completes.** L02 called it an
advantage (robust), L12 a cost (illegible), L13 a cost (it forgets). One fact
underneath all three: **a concept has no address** — you cannot destroy it
selectively, find it, or protect it. Three lectures, three verdicts, no
cross-reference. See [[local-vs-distributed-representation]].

**Every fix is a resource trade, which is the real lesson of
[[continual-learning-strategies]].** Regularisation pays in **capacity**,
dynamic architectures in **size**, replay in **memory**. Nothing is free, and
hybrids work only because the currencies differ.

**The best biological argument in the module arrives on page 2 and gets one
line.** *"Disrupting slow wave sleep impairs long-term memory consolidation"* is
the only **falsifiable experimental result** in thirteen lectures — everything
else is anatomy, description or assertion. And its status is unusual: replay
buffers were invented for optimiser reasons, and the neuroscience matched
afterwards. **Convergence from both directions**, a seventh and strongest
standard on the [[ann-brain-correspondence]] scorecard.

**And two gaps become permanent.** [[intrinsic-motivation|Reinforcement learning]]
is drawn completely — agent, environment, action selection, external and internal
reward — for the fourteenth time, still with no algorithm. **Swarm and collective
intelligence never appear at all.** For a module called *Bio-Inspired AI*, the
absence of ant colonies, particle swarms and flocking across thirteen lectures is
the single most surprising fact in this wiki.

**The closing line is the right one.** *"Current models: far from providing
flexibility, robustness & scalability."* After thirteen lectures of asserted
correspondences, the module grades itself honestly exactly once, on the last page.

## A recurring representational trick

Not named by any lecture, but now visible **seven** times: **a continuous quantity
is represented by position in an ordered population.**

| Lecture | Quantity | Encoded by |
|---|---|---|
| L01 | location in the environment | [[place-cells]] |
| L04 | position in a feature space | the winning unit of a [[self-organising-map]] |
| L05 | sound frequency | [[tonotopic-representation]] |
| L05 | interaural delay | which coincidence detector fires ([[jeffress-model]]) |
| L06 | edge angle | which V1 cell fires hardest ([[orientation-tuning]]) |
| L07 | position in space | column in the [[superior-colliculus]]'s topographic map — and *several maps stacked in register* |
| L10 | **which joints belong together** | [[openpose]]'s **part affinity fields** |

Neighbouring values are handled by neighbouring cells, so topology is preserved.
In the SOM this is *learned*; everywhere else it is built in by anatomy. This is
arguably the module's most consistent commitment, and no lecture states it.

## Cross-cutting themes

These are the threads to watch as later lectures arrive:

- **Static vs temporal.** L02's entire second half is an escape from
  time-blindness. L05 is where it pays off — ITD cannot be rate-coded at all.
- **Learn or implement?** New with L05. [[hybrid-architecture]] asks which parts
  of a system should be fitted from data and which should simply be written
  down. The first push-back against learning-by-default.
- **Local vs global information.** [[hebbian-learning]] and [[stdp]] use only
  information available at the synapse. Bio-inspired methods generally prefer
  local rules; watch whether later lectures keep this discipline.
- **Level of abstraction.** L01 works at the level of behaviour, L02 at the
  level of a single synapse. The module has not yet said which level it thinks
  intelligence lives at.
- **One unit vs many.** L01 promises *communicate and cooperate*; L02 delivers
  only single units and pairwise synapses. Collective intelligence is still
  owed — though L04's [[self-organising-map]] is the first model where units
  *compete and cooperate laterally* rather than just feeding forward.
- **Grounding.** New with L04. [[symbol-grounding]] is the question of how a
  representation acquires content at all, and it is the first properly
  philosophical problem the module has posed.
- **Representation.** [[local-vs-distributed-representation]] is posed in L02 as
  a coding question, and L04 takes a side on it at the level of whole faculties
  (*involves the whole brain*). Likely to return in evolutionary encodings.

## Open threads

- **Local vs global learning — half resolved at L06.**
  [[dynamic-weight-sharing]] is a genuinely local rule that provably implements
  a global constraint, and L06 states outright that CNNs need
  [[weight-sharing]] *which real neurons cannot do*. But it fixes **sharing**,
  not **credit assignment through depth**. Backpropagation is still unaccounted
  for. See [[ann-brain-correspondence]].
- **Reinforcement learning is named, not developed.** [[learning-paradigms]]
  defines it and *reward ↔ dopamine* appears once. No algorithm yet — and L05's
  robot turns its head without any notion of reward. **L08 sharpens this to a
  hole**: a whole lecture on agents acting in a world, whose own disadvantage
  list says *“difficult to make a reactive agent that learns globally”*, without
  naming the field that exists to fix it.
- ~~**[[convolutional-network]] is named once**~~ — **resolved at L06**, which
  gives LeNet-5, the convolution arithmetic, [[pooling]], and the cortical
  derivation. Three lectures late, but complete.
- ~~**Evolution and swarm intelligence are absent from a module called
  Bio-Inspired AI**~~ — **half-resolved at L11**, which is evolution in full.
  **Swarm and collective intelligence remain absent** with two lectures to go.

- ~~**No activation derivatives anywhere**, though [[backpropagation]] requires
  them~~ — **resolved at L10**, which gives `f' = f(1 − f)` for the sigmoid. The
  chain rule is now completable from the module's own material. `tanh`, ReLU and
  softmax derivatives remain absent, and with them any account of why ReLU
  displaced the sigmoid — see [[activation-function]].
- **L01's navigation example is dropped** and never modelled.
- **Evolution and swarm intelligence have not appeared at all**, despite being
  in the module title's scope. **Nine** lectures in, this is the largest gap
  against the stated topic. Four lectures remain.
- **"Fully connected" architecture** is drawn in L02 and never developed.
- ~~**Attention is used but never defined**~~ — **resolved at L09**, which
  defines the psychological construct in full. But L09's attention and [[gpt]]'s
  are **different mechanisms sharing a name**, and the module never says so. See
  [[attention]].
- ~~**Dorsal/ventral appear swapped in L04**~~ — **confirmed at L06**, which
  gives *dorsal = where, ventral = what*. The module contradicts itself and the
  later lecture is right. See [[dual-stream-hypothesis]] and
  [[two-visual-streams]]. The first error caught by the module against itself.
- **Grounding is claimed for models that ground sign in sign.**
  [[cross-modal-stimuli-prediction]] and [[multi-layer-associator]] relate one
  modality to another; only [[imitation-network]] involves a world. See
  [[symbol-grounding]].
- **Elevation is never solved.** [[azimuth-and-elevation]] promises two
  coordinates; every L05 model returns one. Front/back ambiguity is not raised
  either.
- **The [[cocktail-party-problem]] opens L05 and is not solved** — every system
  in the lecture assumes a single source.
- **Still no learning rule for a spiking network.** L05 sidesteps it by
  hand-wiring the spiking stage from anatomy; L06 drops spikes entirely and
  works in rates without comment. See [[spiking-neural-network]] and
  [[neural-coding]].
- **No training procedure for the CNN.** L06 specifies the forward pass in full
  and never mentions how the filters are learned — which is awkward, since the
  gradient of a shared weight is precisely the non-local operation the lecture
  objects to.
- **New: "Hebbian" has quietly come to mean "local".** Three rules across L04
  and L06 are called Hebbian and are not correlational. Recorded on
  [[hebbian-learning]]. Watch whether later lectures continue the usage.

## Navigation

- [[index]] — full catalogue of pages.
- [[log]] — what was ingested when.
