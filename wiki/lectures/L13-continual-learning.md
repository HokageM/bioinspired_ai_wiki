---
title: "Lecture 13 ? Continual Learning"
type: lecture
sources: [L13]
tags: [continual-learning, learning, memory, language, foundations]
updated: 2026-09-21
---

# L13 ? Continual Learning

Source: `raw/lectures/BioinspiredAIWdh13.pdf`, 8 pages, dated 18.02.2024.
The final lecture of the module.

It is also the **best** lecture in the module, and by a clear margin. It states a
problem precisely, gives four families of solution with their costs, writes out a
full algorithm, **supplies evaluation metrics** ? the first and only quantitative
anything in thirteen lectures ? and ends by saying the field has not solved it.

## The problem

Learning strategies, contrasted:

| | **Batch learning** | **Continual learning** |
|---|---|---|
| Tasks | **fixed set** | **dynamic number** |
| Data | all training data available | **no access to previously seen samples** |
| Phases | train, then test | **no distinction between training and test** |
| New task | **prone to catastrophic forgetting or interference** | learn without interfering with existing knowledge |

[[continual-learning|Continual learning]] (lifelong learning) is then:

> - **Ability to continually acquire, fine-tune and transfer new knowledge and
>   skills over time**
> - **Overcome catastrophic forgetting when learning from changing input
>   distributions**
> - **No retraining from scratch and no re-access to previously seen data**
> - **Constrained computational and memory resources**

Four requirements, each of which independently rules out the standard recipe.

### Catastrophic forgetting

> Training a model with new information **interferes with previously learned
> knowledge**. Performance degradation (interference) all the way to a **complete
> overwriting (forgetting)** of old knowledge with new one.
>
> **Catastrophic forgetting affects all connectionist models.**

That last clause is the important one and is stated as a universal. See
[[catastrophic-forgetting]] for why it follows directly from
[[local-vs-distributed-representation|distributed representation]] ? the module's
long-running trade-off, appearing for the third time with a third sign.

### The stability?plasticity dilemma

> **Stability:** preserve and consolidate structured knowledge ? learn in a slow,
> sustainable manner ? **generalization**
> **Plasticity:** adapt quickly to changes in the environment ? efficiently learn
> from novel sensory input ? **specialization**

The framing that organises the whole lecture: [[stability-plasticity-dilemma]].

## Four strategies

[[continual-learning-strategies]] gives the full treatment. In brief:

| Strategy | Mechanism | Cost | Example |
|---|---|---|---|
| **Regularisation** | constrain weight updates with extra loss terms | **fixed model capacity** | [[elastic-weight-consolidation]] |
| **Dynamic architectures** | modular structural change in response to novel input | **capacity and cost grow with training** | [[progressive-neural-network]] |
| **Rehearsal / replay** | retrain on stored or generated old samples | **memory and cost grow with #tasks** | [[carl]] |
| **Hybrid** | all three at once | ? | [[growing-dual-memory]] |

Each strategy's cost is highlighted in orange in the notes. The pattern is worth
saying plainly: **every strategy buys retention with a resource, and they differ
only in which resource.**

### Applied to a network

> a) **Retraining with regularization** ? b) **Training with network expansion** ?
> c) **Selective network retraining and expansion**

Three diagrams of the same network with progressively more of it protected and
more of it added. (c) is the combination.

### The biological argument

Rehearsal gets the lecture's one piece of real neuroscience:

> **Memory replay:** memory reactivation and replay for memory consolidation.
> **Cortico-hippocampal interaction**, during sleep and awake phase.
> **Disrupting slow wave sleep impairs long-term memory consolidation.**
> Learn also in the absence of sensory input.

See [[memory-replay]]. The sleep-disruption result is the **first falsifiable
experimental finding cited anywhere in the module** ? a manipulation with an
outcome, rather than an anatomical description.

## Evaluating continual learning

> **Average accuracy:** `A_k = (1/k) ? ?_{j=1..k} a_kj`
> **Forgetting:** `f_j^k = max_{m ? {1..k?1}} (a_mj) ? a_kj`

`a_kj` = accuracy on task `j` after training on task `k`.

> [!success] The module's oldest gap, closed in the last lecture
> ~~No lecture in this module defines a single evaluation measure.~~ Twelve
> lectures asserted that systems *worked* without saying how anyone would know.
> L13 writes down two metrics, and `f_j^k` is well-designed: it measures
> **forgetting against the model's own best past self**, not against a baseline,
> which is exactly the right control for the phenomenon.
>
> Still no *numbers* ? no lecture in the module reports a result. But there is now
> a definition of what a result would be. See
> [[continual-learning-metrics]].

## Growing Dual-Memory networks

The lecture's flagship system, [[growing-dual-memory|GDM]]: a three-level stack of
input image ? **convolutional feature extraction** ? **G-EM episodic memory
(instance level)** ? **G-SM semantic memory (category level)**, with **temporal
synapses** giving spatiotemporal (context) learning via a recurrent
[[gwr-network|GWR]].

Episodic below semantic, instances below categories, feeding upward ? with
[[memory-replay|replay]] between them. It is
[[complementary-learning-systems|complementary learning systems theory]] built as
an architecture, and it is the module's most complete answer to anything.

## GWR, finally specified

L10 introduced [[gwr-network|Grow-When-Required]] as the network that explains
L08's unnamed "self-organised network of behaviours", but never gave its
algorithm. **L13 gives it in full** ? fifteen numbered steps, two cases, every
update equation. See [[gwr-network]] for the complete pseudocode.

> [!success] The L10 stub is resolved
> ~~GWR is named and used in L10 but never specified: no growth criterion, no
> update rule, no pruning rule.~~ L13 supplies all three: growth when
> `a(t) < a_th` **and** `h_b < h_th` (poor fit **and** well-habituated), updates
> scaled by the firing counter, and pruning by edge age.

The key insight the pseudocode encodes: growth is gated by **two** conditions.
A poorly-fitting input alone does not add a neuron ? the winner must *also* be a
practised unit that should have known better. That conjunction is what stops the
network growing a neuron per sample, and neither L10 nor L13's prose mentions it.

[[associative-gwr]] adds classification: each neuron keeps a list of the classes
it has won, and a novel input is labelled by its BMU's most frequent class.

> **Expandable / shrinkable networks ? Hebbian-like structural plasticity ?
> input-driven self-organization ? neurogenesis: neural activation, habituation.**

## Continual language learning

[[continual-language-learning|CLL]]: question answering, text
classification/sentiment analysis, machine translation.

Two challenges, each with a limitation and a current solution:

| Challenge | State of the art | Limitation | Current solution |
|---|---|---|---|
| **Word/sentence representation** | embeddings pretrained on massive cross-domain general-purpose corpora | performance drops on domain-specific downstream tasks, and **vocabulary changes over time and domain** | meta-learning of prior knowledge to generate improved new-domain embeddings |
| **Fine-tuning language models** | transformer LMs pretrained on large corpora, then fine-tuned per task | **knowledge about all tasks must be present prior to designing the model** | (1) view all tasks through the lens of QA; (2) feed domain and task information as input, not just sequences |

The second limitation is the sharpest sentence in the lecture. The pretrain?
fine-tune recipe is *definitionally* not continual: you must enumerate the tasks
before you build the model, which is the batch assumption wearing a new hat.

SotA: **BERT** ? *Bidirectional Encoder Representations from Transformers* ?
**step 1: unsupervised pretraining, step 2: supervised fine-tuning**. See [[bert]].

## Future directions and related paradigms

> **CLL future directions (bio-inspired):**
> - learning high-level language concepts and compositional structures
>   (**compositional / systematic generalization**)
> - storing semantic concepts from previously learned tasks in a compact fashion
>   (**knowledge distillation**)
> - recombining semantic concepts when exposed to novel tasks
>   (**abductive reasoning**)

Four related paradigms:

| Paradigm | Content |
|---|---|
| **[[developmental-and-curriculum-learning|Developmental & curriculum learning]]** | critical periods of learning; early experiences are the most influential; curriculum of increasingly complex tasks. Sketch: plasticity falls as complexity rises. |
| **[[transfer-learning|Multi-task transfer learning]]** | apply knowledge in one domain to a novel task; positive and negative transfer; forward and backward transfer |
| **[[intrinsic-motivation|Curiosity and intrinsic motivation]]** | select strategies that maximise reward; empirical process of exploration; **self-generation of a learning curriculum**. Diagram: environment ? external reward ? agent's strategy/action selection ? internal reward ? intrinsic motivation |
| **[[multisensory-integration|Crossmodal learning]]** | multisensory integration and crossmodal enhancement; dynamic process across a lifespan; address sensory uncertainty and conflict resolution |

> [!warning] Reinforcement learning, appearance fourteen and final
> The curiosity diagram is an **actor?critic reinforcement learner with an
> intrinsic reward signal**, drawn completely ? agent, environment, action
> selection, external reward, internal reward. The module has now drawn RL's
> architecture without ever naming its algorithm, across fourteen mentions and
> eleven lectures. The last chance to define it is spent on a variant of it.

## Summary (the lecture's own)

> - **CL is the ability to incrementally learn over time without forgetting**
> - stability?plasticity dilemma
> - **Bio: Hebbian learning & complementary systems theory**
> - overcome catastrophic forgetting in NN
> - continual object recognition; CL on sequences with **Gamma-GWR**; **GDM: hybrid
>   model with memory replay**
> - NLP: CLL and challenges
> - **Current models: far from providing flexibility, robustness & scalability**
> - **Basis for modelling higher-level cognitive functions in AI**

## Verified from the source

- The GWR pseudocode is transcribed step by step from p5 and is complete ?
  initialisation, BMU/SBMU selection, edge creation, activation, both cases, the
  habituation counters, edge ageing and pruning.
- Both evaluation metrics are transcribed exactly as written.
- The strategy-cost pairings are the notes' own orange-highlighted annotations,
  not inference.

## Errors and things to watch

- **`f_j^k` is written `f_j^k = max_{m?{1..k?1}}(a_mj) ? a_kj`.** The subscript and
  superscript conventions differ from `a_kj`'s; read `f_j^k` as *forgetting on task
  `j` measured after task `k`*.
- **"Catastrophic forgetting affects all connectionist models"** is stated
  without qualification. It is true of models with **shared distributed**
  parameters; a [[local-vs-distributed-representation|localist]] network with
  disjoint units per task does not forget, which is precisely why the dynamic
  architecture strategy works. The lecture's own strategy (b) is a counterexample
  to its own universal claim.
- The **GDM diagram's arrows all point upward** ? the same feed-forward-only
  drawing problem flagged in [[top-down-modulation]] for L12 ? yet replay in a
  dual-memory system is **downward** by definition: semantic memory regenerates
  episodic samples. The mechanism the page is about is the one the diagram omits.
- **"Rehearsal: explicitly stored training samples"** contradicts continual
  learning's own stated requirement of **"no access to previously seen training
  samples"**. The lecture lists rehearsal as a CL strategy without noting that it
  violates the definition given two pages earlier ? this is exactly why
  *pseudo*-rehearsal and generative replay exist, and the notes give those next
  without drawing the connection.

## Unclear in the source

- **`a_th` and `h_th`, the two GWR thresholds, are never given values or a rule for
  choosing them.** They control the growth rate and therefore everything about the
  network's size, and the lecture says nothing about setting them.
- **`?_b`, `?_n`, `?_b`, `?_n`, `?`, `?_max`** ? six more GWR constants, all
  unspecified.
- **"CaRL"** is given as the rehearsal example with no expansion of the acronym and
  no description. Probably *iCaRL* (Incremental Classifier and Representation
  Learning) [external], but the notes write "CaRL", so this is recorded as a
  [[carl|stub]].
- **Elastic Weight Consolidation** and **Progressive NN** are named as examples
  only; no mechanism for either.
- **"Meta-learning of prior knowledge"** is offered as the current solution to the
  embedding problem, with no definition of meta-learning anywhere in the module.
- **"Knowledge distillation"** and **"abductive reasoning"** are named as future
  directions, undefined.
- **"Temporal synapses"** in the GDM diagram: the box shows a neuron with a `t?1`
  connection, so it is a recurrent link, but the relationship to
  [[gamma-gwr|Gamma-GWR]]'s temporal context descriptors is not stated.
- Whether **G-SM learns from G-EM's activations or from replayed samples** is not
  said, and this is the central design question of the architecture.
