---
type: lecture
lecture: L04
title: Bio-inspired Embodied Language Processing
source: raw/lectures/BioinspiredAIWdh4.pdf
pages: 10
dates_on_pages: [2024-01-26, 2024-01-31]
tags: [language, embodiment, self-organisation, nlp]
---

# L04 — Bio-inspired Embodied Language Processing

> Source: `raw/lectures/BioinspiredAIWdh4.pdf`, 10 pages, all marked **L4**.
> Two session dates appear: p1–p2 are 26.01.2024, p3–p10 are 31.01.2024.

## Summary

The first lecture that leaves the neuron behind. Its question is not *how does a
unit compute* but *how does meaning get into a machine at all*.

The argument runs in four moves:

1. **Language is universal in humans and unique to humans**, and it is
   *compositional*. The lecture's causal claim is that universality is a
   **consequence of brain architecture** — the brain evolved language-processing
   systems, so every human society has language.
2. **Where is it?** A history lesson: [[phrenology]] guessed, [[paul-broca]] and
   [[carl-wernicke]] found out by autopsy. This gives the classical
   [[language-areas-of-the-brain]] picture — frontal lobe produces, temporal
   lobe comprehends.
3. **That picture is then rejected.** Modern imaging shows language is
   *distributed*, *embodied*, and involves the whole brain —
   [[embodied-language-representation]]. The [[dual-stream-hypothesis]] replaces
   the two-box model.
4. **So how do you build it?** Three computational answers, in increasing
   distance from biology: the [[multi-layer-associator]] (Hebbian, brain-shaped),
   the [[self-organising-map]] (unsupervised, topology-preserving), the
   [[imitation-network]] (supervised, embodied) — and then the frankly
   non-biological [[word2vec]], [[gpt]], [[hubert]].

The lecture's real subject, never named as such in the notes but stated
plainly in the closing summary, is **[[symbol-grounding]]**: how a word gets
attached to a thing.

## Key ideas

### Language

- **Universal** in human society, **unique** to humans.
- **Compositional** — see [[compositionality-of-language]].
- ⇒ *Universality of language is a consequence of the fact that the human brain
  has evolved language-processing systems.*

### Psychological / neurobiological models

| Approach | Method | Verdict |
|---|---|---|
| [[phrenology]] / craniology | skull bumps taken to reflect enlarged brain areas | wrong, but located language anteriorly |
| Empiricism ([[paul-broca]]) | anatomical inspection of the brain | correct |

### The classical two-area model

- **[[paul-broca]]'s area** — *motor & production*. Damage ⇒ **Broca's aphasia**:
  impaired production, sparse and non-fluent speech, deficits of intonation and
  stress, lack of grammatical structure, omission of function words.
- **[[carl-wernicke]]'s area** — *auditory & comprehension*, left superior
  temporal lobe, "where auditory words are stored". Damage ⇒ **Wernicke's
  aphasia**: impaired comprehension.
- A connection runs between Wernicke and Broca.

> [!note] Self-correction in the source
> On p2 the temporal-lobe line was first written "impaired language production",
> then struck through and rewritten "impaired language comprehension". The
> correction is right. Recorded because it marks exactly the point students
> conflate.

### Functions, as pathways

- **Comprehension** maps *sound → meaning*: speech → words → structures →
  meanings. Labelled **dorsal**.
- **Production** maps *meaning → speech*: meaning → grammatical encoding →
  phonological encoding → articulation. Labelled **ventral**.
- Early models were **rejected** for *anatomic and linguistic
  underspecification*; imaging tools caused a **paradigm shift**.

> [!warning] Dorsal/ventral labels are swapped — **confirmed at L06**
> The notes attach *dorsal* to comprehension and *ventral* to production. In the
> standard dual-stream account it is the reverse: the **dorsal** stream is the
> sound-to-articulation (production/sensorimotor) route and the **ventral**
> stream is the sound-to-meaning (comprehension) route. `[external]`
>
> **[[L06-hierarchical-vision]] confirms this from inside the module**: p4 gives
> *Dorsal = Where, Ventral = What*, p5 backs it with projection targets and
> lesion studies. Meaning is *what*, so L04's assignment is wrong and the two
> lectures contradict each other.
> Kept as written, flagged here and on [[dual-stream-hypothesis]] and
> [[two-visual-streams]].

### The embodied picture

- Language processing: several pathways; **re-uses** parts of the brain in both
  production and comprehension; spatial and temporal **hierarchical abstraction**.
- Language representation: **distributed semantic maps**, **embodied** in
  multi-modal perception, **involves the whole brain**.
- e.g. **"shark"** activates visual areas — because you have seen one.
- ⇒ **word-webs**. Different areas are active for words denoting objects,
  subjects, body parts.

### Models

- [[multi-layer-associator]] — auditory word recognition and motor control of
  speech; mimics neuroanatomical connectivity; A1 (morpheme / auditory form)
  ⇄ M1 (articulatory form), 25×25 cells per layer, sparse, trained with the
  Hebbian rule `Δw_ij = α·x_i·x_j`.
- [[self-organising-map]] — the lecture's centrepiece, given a full algorithm.
- [[cross-modal-stimuli-prediction]] — a SOM in the middle of visual and
  auditory encode/decode paths; concepts activate *latent sensory
  representations*.
- [[imitation-network]] — epigenetic robot, demonstrator/imitator stick figures,
  basic grounding then [[transfer-learning]] to higher-order grounding.
- [[word2vec]] (CBOW, skip-gram), [[gpt]], [[hubert]].

### Results

- Language representations allow composing complex linguistic constructions from
  primitives ⇒ **compositional language structure**.
- Grounding: *result not just accurate but also contextually correct.*
- Evaluation shows the model can infer **unobserved compositions** similarly to
  humans — and is even likely to make **similar mistakes**.

### Closing summary (p10, near-verbatim)

- Models for explaining human NLP: sensor-to-actuator mapping; semantic
  association.
- Grounding language in action → language acquisition / knowledge acquisition.
- [[hebbian-learning]], SOM learning, statistical learning.
- Conceptual representations for stimulus prediction.
- Classic NLP improved by brain-inspired representation.

## New pages created

Concepts — [[compositionality-of-language]], [[phrenology]],
[[language-areas-of-the-brain]], [[dual-stream-hypothesis]],
[[embodied-language-representation]], [[symbol-grounding]],
[[transfer-learning]], [[word-embedding]], [[self-supervised-learning]],
[[cross-modal-stimuli-prediction]]

Systems — [[self-organising-map]], [[multi-layer-associator]], [[word2vec]],
[[gpt]], [[hubert]], [[imitation-network]]

Entities — [[paul-broca]], [[carl-wernicke]], [[teuvo-kohonen]]

## Pages updated

- [[hebbian-learning]] — L04 gives the rule a second, concrete application
  (associator, SOM neighbourhood) and a coefficient name `α`.
- [[local-vs-distributed-representation]] — L04 is the strongest statement of
  distributed representation in the module so far.
- [[learning-paradigms]] — L04 adds **self-supervised** as a fourth paradigm,
  which L03's three-way split did not have.
- [[network-architectures]] — SOM is a new architecture class (competitive,
  topological).
- [[recurrent-neural-network]] / [[gated-recurrent-network]] — GPT is explicitly
  *attention instead of recurrence*; this is the successor to L03's RNN thread.
- [[ann-brain-correspondence]] — L04 sharpens the tension: it offers both a
  biologically-motivated model *and* a transformer, without ranking them.

## Connections

- **The local-learning thread survives.** L02's [[hebbian-learning]] reappears
  twice here as a working algorithm. This is the first lecture where the
  biologically plausible rule is used to build something, rather than being
  described and then abandoned for [[backpropagation]].
- **But the lecture ends on GPT.** The last two models are trained by gradient
  descent on enormous corpora with no biological story at all. The lecture does
  not comment on this drift.
- **Unsupervised learning finally gets an algorithm.** [[learning-paradigms]]
  defined it in L03; the [[self-organising-map]] is the first one the module
  actually runs.
- **L01's [[place-cells]] / [[grid-cells]] are the biological cousin of the SOM**
  — topology-preserving maps in cortex. The lecture does not make this link;
  the wiki does.

## Unclear in the source

- **Dorsal/ventral are swapped** — *confirmed wrong by [[L06-hierarchical-vision]]*,
  which gives dorsal = *where*, ventral = *what* (see warning above).
- **"25 × 25 cells each layer"** is given as *sparseness*. Sparseness is a
  property of activation, not of layer size; the notes conflate grid dimensions
  with sparse coding.
- **The Hebbian rule is written `Δw_ij = α(x_i)(x_j)`** with no decay,
  normalisation or bound — the runaway-growth problem flagged on
  [[hebbian-learning]] is not addressed.
- **`avg min_ij d_ij`** on p5 — "avg" is almost certainly *arg*. Recorded as
  `arg min`.
- **The neighbourhood function `h_u(t)` is never given a form.** Gaussian is the
  usual choice `[external]` but the notes do not say.
- **"Four pairs of motor neurons"** — the imitation network's output layer is
  described only by this phrase; no architecture diagram survives.
- **The right margin of p6 is cut off** — "Neighbourhoods form ca…" is
  incomplete (almost certainly *categories*).
- **HuBERT's "offline clustering"** step is named but the clustering algorithm
  is not.
- **GPT's self-attention is never defined** — only "attention instead of
  recurrence".

See [[index]] · [[overview]] · [[log]]
