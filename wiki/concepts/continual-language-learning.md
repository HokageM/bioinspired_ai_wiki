---
title: Continual language learning (CLL)
type: concept
sources: [L13]
tags: [continual-learning, language, nlp, transformer]
updated: 2026-09-21
---

# Continual language learning

> **Applications & tasks:** question answering ? text classification / sentiment
> analysis ? machine translation. **NLP: natural language processing.**

[[continual-learning|Continual learning]] applied to language, and the lecture's
route from the module's oldest topic to its newest.

## Two challenges

### 1. Word / sentence representation

> **State of the art:** language embeddings are usually generated from pretraining
> on massive cross-domain, general-purpose corpora.
> **(?) Limitation:** decreasing performance on domain-specific downstream tasks,
> and with respect to **changes of vocabulary in time and domain**.
> **Current solution:** **meta-learning** of prior knowledge in order to generate
> improved new-domain embeddings.

The vocabulary point is the genuinely continual one and the only place the module
acknowledges that **language itself changes**. A [[word2vec|fixed embedding]]
assumes a closed vocabulary with stable meanings; new words appear, old words
shift, and domains use the same word differently. The
[[catastrophic-forgetting|distribution shift]] is in the *input space itself*,
not just the task.

> [!warning] "Meta-learning" is undefined here and everywhere in the module
> Offered as the current solution and never explained. `stub-by-source`.

### 2. Fine-tuning language models

> **Transformer-based language models are pretrained on large-scale corpora and
> then have to be fine-tuned, tailored to the task at hand.**
> **(?) Limitation: knowledge about all tasks must be present prior to designing
> the model.**
> **Current solution:** (1) all tasks viewed through the lens of **QA (question
> answering)**; (2) not only sequences, but also **info about domain and task** are
> used as input for general-purpose models.

> [!note] This is the sharpest sentence in the lecture
> *"Knowledge about all tasks must be present prior to designing the model"* is
> the **batch assumption** ([[continual-learning]]'s "fixed set of tasks") hiding
> inside the pretrain-then-fine-tune recipe. You must enumerate the tasks to size
> the heads and choose the objectives. The dominant paradigm in NLP is, by
> construction, not continual.
>
> Both solutions attack this the same way: **stop encoding the task in the
> architecture and move it into the input.** Casting everything as QA makes one
> model shape serve all tasks; feeding domain and task descriptions as text makes
> a new task a new *input* rather than a new *model*. A task set that no longer
> has to be known in advance is exactly what CL requires.
>
> This is also, unremarked, a description of what [[gpt|prompting]] turned out to
> be ? and L12 discussed prompting without connecting it to this.

## Future directions (bio-inspired)

> - learning high-level language concepts and compositional structures
>   (**compositional / systematic generalization**)
> - storing semantic concepts from previously learned tasks in a compact fashion
>   (**knowledge distillation**)
> - recombining semantic concepts when exposed to novel tasks
>   (**abductive reasoning**)

All three are named and none defined. They form a coherent programme, though ?
*build parts, compress them, recombine them* ? and it is close to what
[[symbolic-ai]] claimed to offer in L12: compositional structure over reusable
discrete concepts. The module's last substantive page quietly asks for the thing
its second-to-last page said neural networks lack, without noticing.

## See also

- [[continual-learning]] ? [[bert]] ? [[gpt]] ? [[word2vec]] ?
  [[L13-continual-learning]]
