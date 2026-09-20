---
title: "L12 ? Neuro-Symbolic and Explainable AI Systems"
type: lecture
lecture: 12
sources: [L12]
tags: [explainability, hybrid, representation, language]
updated: 2026-09-21
---

# L12 ? Neuro-Symbolic and Explainable AI Systems

**Source:** `raw/lectures/BioinspiredAIWdh12.pdf`, 9 pages, dated 17.02.2024.

The module turns on itself. Eleven lectures built neural systems; L12 asks what
any of them actually **know**, and whether that can be said out loud.

## The two traditions

> **Connectionist and NN** ? neural networks emulate brain's plasticity and
> functional networks. Massively parallel processing. Plethora of parameter
> tuning. **Function approximation vs cognition.**
>
> **Symbolic AI** ? `IF ? AND ? AND ? THEN ?`. Strong AI research in inference
> systems to model human recognition. Symbolic representations and their
> manipulation; symbolic understanding. Sequential list processing, recursion
> (**Prolog, Lisp**). **Small domains only.**

> [!note] "Function approximation vs cognition" is the whole module in four words
> It is the charge that has hung over every architecture since L03, stated here
> for the first time as a **question about the neural approach itself**, not about
> a particular model. A network that fits a function need not be doing anything
> cognitive, and eleven lectures of
> [[ann-brain-correspondence|correspondence claims]] have never had a way to tell
> the difference.
>
> Symbolic AI's answering weakness is equally blunt: *"small domains only."*

See [[symbolic-ai]] and [[neural-symbolic-integration]].

## Why hybridise

> **Brain as a hybrid system** supporting different forms of processing:
> **signals, symbols, structures, knowledge.**
> Hybrid systems in AI and knowledge engineering for **increasing performance**.
> Hybrid processing in cognitive science and cognitive neuroscience for
> **plausible cognitive models**.
> **Explainable AI (XAI) can use symbolic representations.**

Three distinct motivations ? performance, plausibility, explainability ? and they
are not the same goal. The lecture lists them together and pursues mainly the
third.

The worked argument for why neither side suffices:

> **Cognition is not precise** (*"go close to the table"*).
> Different forms of properties and form of processing: multimodal integration of
> cognitive processing; speech/language integration (signals & concepts);
> information retrieval (text & images).

> [!note] *"Go close to the table"* is the best example in the lecture
> A symbolic planner needs a number. A neural controller has no notion of *close*
> as a **concept** that transfers to a chair. The instruction is symbolic in form
> and continuous in execution, which is exactly the join the lecture is arguing
> for ? and it is the same problem as
> [[embodied-language-representation|grounding]] from L04, approached from the
> symbolic side.

## The comparison table

Reproduced in full on [[neural-symbolic-integration]]. Its most important row is
**distributed vs localist**, which is [[local-vs-distributed-representation]]
from L02 reappearing as the axis dividing two entire research traditions.

## Architectures

[[hybrid-integration-architectures]] ? the taxonomy: **transfer** architectures
(knowledge moves between the two) versus **processing** architectures, the latter
graded by coupling: **loose** (separable, unidirectional), **tight** (separable,
bidirectional), **integration** (fully embedded, *"work together"*).

The three-level knowledge-technology stack:

> a) **Symbolic knowledge and understanding**
> b) **Neural/statistical knowledge representation**
> c) **Sensory input from several modalities** (audio, vision, etc.)

drawn with an arrow running **upward** from sensors through neural
representations to symbols.

## Knowledge extraction

Four methods, in increasing order of what they reveal:

| Method | Reads | Page |
|---|---|---|
| Weight-based transfer | the **weights** | [[weight-based-transfer]] |
| Hinton diagram | the weights, **visually** | [[hinton-diagram]] |
| Activation clustering | the **internal representations** | [[knowledge-extraction]] |
| Automata extraction | the **dynamics** | [[automata-extraction]], [[preference-moore-machine]] |

The progression is driven by an argument the lecture makes explicitly:

> **Weights represent knowledge at a very detailed level.**
> **Activation values represent more integrated knowledge for particular
> pattern.**
> **Weights ? knowledge statically. Processing is needed to represent knowledge
> more dynamically.**

[[transducer-network]] is the system all of this is demonstrated on.

## Explainable deep learning

[[levels-of-abstraction]] ? *pixel ? edges ? shapes ? objects*, *character ? word
? sentence ? story*, *sound ? phone ? phoneme ? word*.

[[class-activation-map]] ? *"which area was used to classify an image."*

[[layer-wise-relevance-propagation]] ? the only XAI method in the lecture given
as an equation.

And [[gpt|ChatGPT]] as an explanation generator, with five strengths and five
weaknesses.

## What L12 changes

**Explanation becomes a design goal.** Every lecture through L11 evaluated
architectures by what they could **do**. L12 evaluates them by whether a person
can find out **why** they did it, and that is a different objective which nothing
in the previous eleven lectures optimises for.

**The distributed representation is reframed as a cost.** L02 presented
[[local-vs-distributed-representation|distributed codes]] as the superior
option ? robust, efficient, generalising. L12 states the bill:
*"distributed representations are difficult to understand and modify."*
Ten lectures of benefit, one of cost.

**Two kinds of knowledge, and the module has only ever built one.**
*Signals, symbols, structures, knowledge* ? the module has spent eleven lectures
on the first and reaches the second in its penultimate session.

## Verified from the source

- LRP: `R_j = ?_k ( z_jk / ?_j z_jk ) R_k`, with `z_jk = a_j ? w_jk`,
  `a_j` = input activation to neuron `j`, `w_jk` = weight from `j` to `k`.
- Preference Moore machine: `I = [0,1]^n`, `S = [0,1]^p`, `Q = [0,1]^m`, with the
  rule **IF state is `a` and input is `b` THEN go to state `x` and output is
  `y`**.
- The Hinton diagram convention: **black = negative weights, white = positive
  weights**.
- The stepwise learning order in the transducer network: high error equally
  distributed ? frequent noun groups ? less frequent prepositional groups, verb
  groups later ? sequential context later ? **exceptions last**.
- Rule extraction produces **N-of-M rules** by *"grouping, elimination and
  clustering of weights"*.

## Errors and things to watch

> [!warning] "Explainable AI can use symbolic representations" is asserted, never argued
> The lecture's premise is that symbols explain and vectors do not. But a
> **symbolic rule extracted from a network is not a description of what the
> network does** ? it is a second model that approximates it. If the rules were
> faithful the network would be redundant; if they are not faithful the
> explanation is wrong. This fidelity/interpretability trade-off is the central
> problem of the field and is not mentioned once.

> [!warning] Two different senses of "explanation" are used interchangeably
> [[class-activation-map|CAM]] and [[layer-wise-relevance-propagation|LRP]]
> answer *which inputs mattered* ? attribution. [[automata-extraction]] and
> [[weight-based-transfer]] answer *what rule is being followed* ? mechanism.
> [[gpt|ChatGPT]] answers *what would a plausible explanation sound like* ? which
> is neither, and is the one the lecture flags as prone to *"hallucination"*.
> All three are presented under one heading.

> [!warning] "Small domains only" is stated about symbolic AI and not revisited
> If symbolic methods do not scale, then extracting symbolic rules from a large
> network inherits the problem. The lecture says as much in passing for the
> Hinton diagram ? *"distribution of weights difficult to show for larger
> networks"* ? and does not generalise it.

## Unclear in the source

- **No attribution.** Hinton is named only via the diagram; LRP, CAM and the
  transducer work are unattributed.
- **N-of-M rules** are named without definition ? *"grouping, elimination and
  clustering of weights"* is the whole description.
- The example rules `a: ?b, c` / `b: ?d, e, f` / `c: ?f, g` are given with no
  statement of what they mean or how they were derived.
- The marginal German gloss *"Umwandler mit Zustand"* (transducer with state)
  clarifies the term; the linguistic example `(n ? ng)`, `(v ? vg)`,
  `(d ? pg)`, `(a ? pg)` is not explained.
- **Critic maps** are mentioned under *"explaining the internals of RL"* ?
  reinforcement learning's **thirteenth** appearance by name, still with no
  algorithm. The word *critic* is a technical RL term and is used without
  definition.
- PCA is invoked for activation analysis without introduction.
- No evaluation numbers, for the twelfth consecutive lecture.

## See also

- [[overview]] ? [[neural-symbolic-integration]] ? [[explainable-ai]] ?
  [[L11-evolutionary-computing]]
