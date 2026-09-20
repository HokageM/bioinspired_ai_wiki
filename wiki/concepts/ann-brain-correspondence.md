---
title: ANN-Brain Correspondence
type: concept
tags: [foundations, neuroscience, neural-networks]
sources: [L03, L04, L05, L06, L07, L09, L11, L12, L13]
created: 2026-09-20
updated: 2026-09-21
status: solid
---

# ANN-Brain Correspondence

The lecture's closing table: **ANNs share some aspects of biological neural
structures** (L03, p6).

## The mapping (L03, p6)

| Artificial | ↔ | Biological |
|---|---|---|
| Nodes / units and weighted connections | ↔ | Neurons and synapses |
| Activation values | ↔ | Action potentials |
| **Backpropagation** | ↔ | **Plasticity** |
| Reward | ↔ | Dopamine |

Named structures: **convolutional networks**, **recurrent networks**.
Named applications: *foundation models, speech processing, crossmodal learning*.

## Assessment

This table is the module's summary answer to L01's requirement 2 — that a
bio-inspired method must *learn, represent and process based on bio-inspired
principles* ([[intelligent-behaviour]]). It is worth reading critically,
because the four rows are not equally solid, and the wiki's other pages
already contain the evidence.

| Row | Status | Why |
|---|---|---|
| Nodes ↔ neurons | **Fair** | The structural abstraction of [[levels-of-abstraction]]; explicitly acknowledged as lossy |
| Activation values ↔ action potentials | **Strained** | An activation value is a *rate*, an AP is an *event*. L02's [[neural-coding]] treats this as the fork between two entire fields. The row silently picks one side |
| Backpropagation ↔ plasticity | **Weak** | See below |
| Reward ↔ dopamine | **Plausible but undeveloped** | Nothing in the module implements it — [[learning-paradigms]] names reinforcement learning and stops |

> [!warning] Tension between L02 and L03
> **Row 3 is in tension with the rest of the wiki.** L02's biological learning
> rules — [[hebbian-learning]] and [[stdp]] — are *local*: a synapse updates
> using only the activity at its two ends. [[backpropagation]] is *global*: each
> weight's update depends on an error computed at the output layer and
> propagated back through every intervening weight. No biological mechanism for
> that reverse pass is given anywhere in L02 or L03.
>
> So "backpropagation ↔ plasticity" equates a mechanism the module has
> established as biological (plasticity) with one it has not (backpropagation).
> The honest version of the row is: *both change connection strengths with
> experience* — which is true, and much weaker than the arrow suggests.
>
> The module does hint at the gap elsewhere: L03 p5 calls the
> [[simple-recurrent-network]] *"biologically more plausible than an MLP"*,
> which only makes sense if MLP training is recognised as implausible.

## Computational form

```text
# the asymmetry that row 3 hides
#
# LOCAL rule (biological — L02):
#     dw[i][j] = f( x[j], y[i] )          # only the two ends of THIS synapse
#     -> implementable by the synapse itself
#
# GLOBAL rule (backpropagation — L03):
#     dw[i][j] = f( delta[i], y[j] )
#     where  delta[i] = sum over k of ( w[k][i] * delta[k] * phi'(a[i]) )
#                                       ^^^^^^^ requires knowing the DOWNSTREAM
#                                               weights and errors
#     -> requires a reverse channel carrying delta backwards along the axon,
#        which is not a thing neurons have.
#
# this is the "weight transport problem". [external]
```

## Where it appears in the module

- [[L03-computational-neural-networks]] — page 6, the closing summary.

## See also

- [[levels-of-abstraction]] — the forward direction of this mapping.
- [[intelligent-behaviour]] — the criterion this table implicitly answers.
- [[backpropagation]], [[hebbian-learning]], [[stdp]] — the rules being equated.
- [[neural-coding]] — why row 2 is not neutral.
- [[learning-paradigms]] — where row 4 would be cashed out, but is not.
- [[self-organising-map]] (L04) — the first local, unsupervised rule in the
  module that actually builds something.
- [[self-supervised-learning]] (L04) — a plausible objective with implausible
  credit assignment.

## What L04 does to this table

L04 is the first lecture to put a biologically-motivated model and a purely
engineered one side by side on the same problem — and it does not adjudicate.

| Model | Learning rule | Local? | Embodied? |
|---|---|---|---|
| [[multi-layer-associator]] | Hebbian | yes | no |
| [[self-organising-map]] | competitive | yes | no |
| [[imitation-network]] | supervised gradient | no | **yes** |
| [[word2vec]] | self-supervised gradient | no | no |
| [[gpt]] | self-supervised gradient | no | no |

Read down the columns and the module's difficulty is visible: **no model is both
local and embodied**, and the two that perform best on real language are
neither. L04 spends seven pages arguing that meaning requires grounding in
multi-modal perception, then presents GPT — text-only, globally trained, with no
brain correspondence at all — without a word of comparison.

This strengthens rather than weakens the page's original complaint. The
*backpropagation ↔ plasticity* equivalence is not just asserted; by L04 it has
become the tacit working assumption, with the biologically plausible rules
demoted to illustrative models and the gradient methods doing the real work.

## A correspondence that holds — L05

L05 supplies the counter-example this page needs. Where *backpropagation ↔
plasticity* is asserted and unsupported, the ITD correspondence is
**demonstrated**:

| Level | Algorithm | Biology |
|---|---|---|
| What is computed | maximum similarity over time shifts | maximum coincidence over delays |
| How | loop over `d_i`, dot product, `argmax` | one detector per delay, all in parallel |
| Where | [[cross-correlation-localisation]] | [[jeffress-model]], MSO |
| Output | the winning shift | the firing detector |

The two are the same function on different hardware — the algorithm *iterates*
the delays, the brainstem *instantiates* them. Nobody has to assert an
equivalence; you can read it off the two procedures.

**What makes this different from row 3 of the table above** is that the
correspondence is at the level of the *computation*, and both sides are fully
specified. "Backpropagation ↔ plasticity" pairs a global algorithm with a local
biological phenomenon and leaves the mapping unstated.

A useful standard falls out of this: a claimed correspondence is worth something
when **both sides can be written down and compared**. By that standard L05
passes, L03 does not, and L04's models ([[word2vec]], [[gpt]]) do not even
attempt it.

> [!note] With a caveat
> `[external]` The delay-line account is solid for the barn owl but contested
> for mammals, where ITD may be encoded by inhibitory timing and population rate
> instead. The lecture presents Jeffress without qualification.

## The bar is raised — L06

L06 is the first lecture to treat biological plausibility as a **constraint to
be satisfied** rather than a badge to be claimed, and it does so twice.

### 1. A correspondence with a documented lineage

The cortex→CNN derivation is not an analogy noticed after the fact. It is a
chain in which each step cites the last, and the *vocabulary itself* is the
evidence:

| Biology | Model | Link |
|---|---|---|
| [[receptive-field]] | kernel of size `r` | the notes use the **same term** for both |
| **simple** cell ([[david-hubel]], [[torsten-wiesel]]) | conv filter | Fukushima's **S**-cells |
| **complex** cell | [[pooling]] | Fukushima's **C**-cells |
| V1 → V2 → V3 → V5 | stacked layers | [[neocognitron]] → [[convolutional-network]] |

[[kunihiko-fukushima]] borrowed Hubel and Wiesel's terms wholesale; the `S` and
`C` in `U_S1`, `U_C1` *stand for* simple and complex. [[yann-lecun]] inherited
the structure. By the standard proposed above — *both sides can be written down
and compared* — this passes, and it is the second such case after L05.

### 2. A plausibility objection, stated and then attacked

> **CNNs require [[weight-sharing]], which real neurons cannot do.**

This is structurally the **same objection** this page has been making against
row 3 of the table: an algorithm that requires information at a synapse which
could not physically be there. Backpropagation needs downstream weights;
weight sharing needs the value of a distant synapse. Both are non-local.

The difference is that L06 *says so itself*, and then does engineering work:

| Repair | Mechanism | Reported result |
|---|---|---|
| [[data-augmentation]] | show all translations simultaneously | *longer training, only small improvement* |
| [[dynamic-weight-sharing]] | lateral connections + local update in a "sleep phase" | no result reported |

And [[dynamic-weight-sharing]] meets the standard. Its rule

```
Δw_i ∝ −( z_i − (1/N) Σ_j z_j ) x  −  γ ( w_i − w_i^init )
```

is **local** (each unit needs only its own activation, its input, and a
population average available over lateral connections) and **provably descends
a global objective** — the variance `Σ(z_i − z_j)²`, whose minimum is exactly
the constraint `w_i = w_j` that weight sharing imposes by fiat. Both sides are
written down. The equivalence is checkable, and it checks out.

**This is the answer to the lint question this page has been carrying since
L03**, though a partial one: the module *does* eventually offer a biologically
plausible alternative to a non-local mechanism — but for **weight sharing**,
not for backpropagation. Credit assignment through depth remains untouched.
The [[convolutional-network]] in L06 is still trained by gradient descent; only
the *sharing* has been made local.

> [!note] Score so far
> Three correspondences in the module now have their mappings written out:
> the [[jeffress-model]] ↔ [[cross-correlation-localisation]] (L05), the visual
> hierarchy ↔ conv/pool stack (L06), and [[dynamic-weight-sharing]] ↔ the
> weight-sharing constraint (L06). All three hold.
> *Backpropagation ↔ plasticity* (L03) still has no mapping at all, and is now
> conspicuously the weakest claim in the module — by the module's own standards.

## Open questions / gaps

- The L03 table is asserted without argument or qualification.
- **Dopamine is mentioned once, in that table, and never again.** No
  reinforcement learning algorithm appears in the module through L06.
- ~~Convolutional networks are named as a structure but never explained.~~
  **Resolved at L06** — see [[convolutional-network]].
- ~~Does any later lecture offer a biologically plausible alternative to
  backpropagation?~~ **Partially resolved at L06.** [[dynamic-weight-sharing]]
  gives a local alternative to *weight sharing*. **Nothing yet addresses
  credit assignment through depth** — row 3 still stands unsupported.
- New: L06 proposes [[dynamic-weight-sharing]] and reports **no performance
  figures for it**, while honestly reporting that the weaker mechanism
  ([[data-augmentation]]) barely helps. Does it work?


## A third and stronger standard ? L07

[[L07-crossmodal-processing]] closes with a claim no earlier lecture makes:

> **Demonstrated behavioural, neurophysiological similarity between models and
> biological systems.**

The [[histogram-based-som]] is validated by checking that it **reproduces the
same phenomena as the biological [[superior-colliculus]]**: depression, the
[[spatial-principle]], enhancement, [[inverse-effectiveness]].

That is a different kind of argument from anything in L03?L06. The model is not
called brain-like because of what it is made of, or because its structure
resembles anatomy. It is tested against **measured empirical signatures** and
shown to exhibit them.

And the signatures are hard to fake. [[inverse-effectiveness]] is subtle,
quantitative and counter-intuitive ? a perfectly reasonable integrator could fail
to show it. Reproducing it is evidence that the *mechanism* is right, not merely
the vocabulary.

### The module's three standards, ranked

| Standard | Lecture | Form of evidence |
|---|---|---|
| **Assertion** | L03 | *backpropagation ? plasticity* ? a table, no mapping |
| **Both sides written down** | L05, L06 | [[jeffress-model]] ? [[cross-correlation-localisation]]; cortical hierarchy ? conv/pool; [[dynamic-weight-sharing]] ? the sharing constraint |
| **Model reproduces measured signatures** | **L07** | enhancement, depression, inverse effectiveness |

The third is the strongest because it is **falsifiable**. The first is not a
claim that could fail.

> [!note] With the usual caveat
> The notes report the validation and show **no data** ? no figure, no number, no
> task, no comparison model. So the wiki records L07 as having the right *form*
> of argument, on the strength of a claim it does not evidence.


## L09: one good method, one bad pun

**The method.** [[human-robot-collaboration]] runs the full loop — observe
humans, model them, implement, test against humans — and the lecture's
conclusion states the standard outright: *"comparison between human behaviour
and modelling / robot performance"*. Together with
[[additive-factors-method]] and [[reaction-time]], L09 is the module's most
methodologically self-aware lecture.

**The pun.** L09 defines attention at length and never mentions transformer
self-attention, although [[gpt]] was introduced in L04 as *attention instead of
recurrence*. The two share a name and almost nothing else — see [[attention]].
This is a **new failure mode** for this page's scorecard: not a weak
correspondence, but a **terminological coincidence mistaken for one**.

| Failure mode | Example |
|---|---|
| Asserted, not shown | L03's *backpropagation ↔ plasticity* |
| Shown, and holds | L05 ITD, L06 hierarchy, L07 signatures |
| **Named alike, unrelated** | **"attention" in L04 and L09** |

And L08 supplies the standing warning that keeps all of this honest: a
[[braitenberg-vehicle]] with four wires supports every behavioural description
we would take as evidence of mind. **Resemblance at the level of description is
free; resemblance at the level of measured quantity is not.**


## L11 ? bio-inspiration one level up

[[L11-evolutionary-computing]] opens with a move no other lecture makes:

> **Powerful problem solvers in nature ? Brain: "wheel" ? evolutionary
> mechanism, that created the human brain.**

Ten lectures copy **the brain**. L11 observes that the brain is itself the
*product* of a search process, and copies **the process that produced it**.

### A fifth kind of correspondence claim

The running scorecard now reads:

| Standard | Example |
|---|---|
| 1. Asserted resemblance | *"backpropagation ? synaptic plasticity"* (L03) |
| 2. Both sides written down | [[jeffress-model|ITD coincidence detection]] (L05) |
| 3. Reproduces measured signatures | [[orientation-tuning]] (L06) |
| 4. **Structure of the signal**, not the mechanism | [[gesture-phases]] (L10) |
| 5. **Copying the generating process, not the product** | **evolutionary computing (L11)** |

And a failure mode list that now has two entries: *"named alike, unrelated"*
(psychological vs transformer [[attention]], L09) and the one L11 adds below.

> [!note] Standard 5 is not weaker than the others ? it is a different claim
> Standards 1?4 assert that an artefact **resembles** something biological. L11
> asserts that a **procedure** is the same procedure. The four pillars ?
> population, diversity, heredity, selection ? are substrate-neutral, so running
> them on candidate solutions is not a metaphor for evolution; it *is* evolution,
> on a different substrate. See [[four-pillars-of-evolution]].
>
> That makes it the most defensible correspondence claim in the module, and the
> one least in need of defending.

### But the operators drift from the biology

> [!warning] A sixth failure mode: "biologically named, engineered anyway"
> - [[recombination|Uniform crossover]] has **no chromosomal counterpart** ? no
>   biological mechanism shuffles genes independently. It is listed beside
>   n-point crossover, whose positional bias *is* inherited from physical
>   chromosome breakage, with no distinction drawn.
> - [[mutation]] is characterised as *"random and unbiased"* and then given four
>   deliberate engineering biases (add connections at zero weight, delete in
>   inverse proportion to weight, `Prob(delete) > Prob(add)`).
> - Every algorithm in the lecture is **haploid**; the biology page states humans
>   have **23 pairs** ([[dna-and-heredity]]).
>
> So the frame is biological and the operators are chosen by what works. That is
> the right engineering decision and it should be labelled, because the biological
> vocabulary otherwise implies a warrant the operators do not have.

## L12 ? an assertion, and a tension the lecture walks into

L12 opens with the sharpest statement of the module's central question:

> **function approximation vs cognition**

? the two traditions' answers to *what is a neural network for*. And it justifies
hybridisation biologically:

> **the brain as a hybrid system supporting signals, symbols, structures,
> knowledge**

> [!warning] Asserted with no evidence, and it is doing real work
> This single clause carries the entire biological argument for neuro-symbolic
> AI. No study, no brain area, no measurement ? unlike L05's
> [[jeffress-model|delay lines]] or L06's [[receptive-field|receptive fields]],
> where a mechanism was at least described. "The brain supports symbols" is the
> conclusion the field would need to establish, offered as its premise.
>
> **Scorecard (sixth standard, failed):** *assertion by vocabulary*. The four
> words signals/symbols/structures/knowledge are the levels of the proposed
> **architecture**, projected onto the brain and then read back as support for
> the architecture.

> [!note] And the lecture's own second half undercuts its first
> L12 argues:
> 1. the brain is a hybrid signal-and-symbol system, **therefore** build hybrid AI;
> 2. neural networks are opaque, **therefore** extract symbols to explain them.
>
> If (1) holds, the brain contains distributed sub-symbolic machinery that is
> exactly as opaque as an ANN, and no amount of symbol extraction explains it.
> The interpretability problem is not an artefact of our models; on the
> lecture's own premise it is a property of the thing being modelled. Neuroscience
> spends its effort on the same problem ? [[hinton-diagram|reading out]] what a
> population code means ? and the module has shown this repeatedly without
> naming it: [[place-cells]], [[tonotopic-representation]],
> [[self-organising-map|SOM maps]] are all cases of researchers *extracting an
> interpretation from a distributed code*.
>
> **Symbol extraction from an ANN and single-unit recording from a cortex are the
> same activity.** The module never says so.

## L13 — the best evidence in the module, and a seventh standard

[[memory-replay]] is where the module's biology finally does real work.

> **Disrupting slow wave sleep impairs long-term memory consolidation.**

> [!success] The first falsifiable experimental result in thirteen lectures
> A **manipulation with a measured outcome**. Intervene on sleep; memory gets
> worse. It could have come out otherwise.
>
> Everything else in the module is anatomy (*the LGN projects to V1*), description
> (*place cells fire at locations*), or assertion (*the brain is a hybrid
> system*). None of those can be wrong in the way this can.

**Seventh standard: convergence from both directions.** Replay buffers were
invented for optimiser reasons — breaking correlation between consecutive samples
— not from neuroscience. The biology was attached afterwards. That makes it
*not* bio-inspiration, and **better** evidence than bio-inspiration: two fields
arrived at the same mechanism from different starting points, under the same
constraint (you cannot learn a new thing from correlated data without damaging an
old one).

The scorecard, complete:

| # | Standard | First met | Verdict |
|---|---|---|---|
| 1 | asserted | L01 | weakest |
| 2 | both sides written down | L04 | |
| 3 | reproduces measured signatures | L06 | |
| 4 | structure of the signal | L10 | |
| 5 | copying the generating process, not the product | L11 | |
| 6 | **assertion by vocabulary** | L12 | **failure mode** |
| 7 | **convergence from both directions** | **L13** | **strongest** |

Plus the two earlier failure modes: *named alike, unrelated* (L09) and
*biologically named, engineered anyway* (L11).

> [!note] And the module's closing claim is the modest one
> *"Current models: far from providing flexibility, robustness & scalability"*
> and *"basis for modelling higher-level cognitive functions in AI"*. After
> thirteen lectures of asserted correspondences, the last page says the systems do
> not work well enough yet. That is the right note to end on, and it is the only
> time the module grades itself.
