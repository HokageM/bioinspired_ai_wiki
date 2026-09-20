---
title: Explainable AI (XAI)
type: concept
sources: [L12]
tags: [explainability, methods, foundations]
updated: 2026-09-21
---

# Explainable AI

> **Explainable AI (XAI) can use symbolic representations.**
> **How can we explain what a network "knows"?**

The module's twelfth lecture is the first to treat this as an objective. Eleven
lectures asked whether an architecture **works**; this one asks whether anyone
can find out **why**.

## Three different questions, presented as one

The lecture's methods answer genuinely different questions, and conflating them
is its main weakness:

| Question | Methods | What you get |
|---|---|---|
| **Which inputs mattered?** | [[class-activation-map]], [[layer-wise-relevance-propagation]] | attribution over one input |
| **What rule is being followed?** | [[weight-based-transfer]], [[automata-extraction]], [[preference-moore-machine]] | a model of the mechanism |
| **What would a plausible explanation sound like?** | [[gpt|ChatGPT]] | fluent text |

The third is not an explanation of the system at all ? it is a **generated
account**, and the lecture's own list of its weaknesses begins with
*"hallucination"*. Presenting it alongside LRP under one heading is a category
error.

> [!note] Attribution and mechanism are not substitutes
> A heatmap showing that a classifier used the correct region tells you *where*
> it looked, not *what it concluded from looking*. It can rule out the
> embarrassing failure ? the network keyed on a watermark ? and it cannot tell
> you the network learned the concept.
>
> Mechanism methods are the stronger claim and scale worse; attribution methods
> scale to any network and say less. The lecture's progression runs from
> mechanism (small recurrent networks) to attribution (deep vision networks) and
> the reason is scale, not preference.

## Model-level and decision-level

A second axis the lecture crosses without marking:

| | Explains | Scales? |
|---|---|---|
| [[hinton-diagram]], [[weight-based-transfer]], [[automata-extraction]] | **the whole model** | no ? *"difficult to show for larger networks"* |
| [[class-activation-map]], [[layer-wise-relevance-propagation]] | **one decision** | yes |

The shift is forced. Once a model has millions of parameters, no depiction of all
of them is an explanation, so the question changes from *what does this model
know* to *why did it say that*. The lecture makes the move and never announces it.

## The unstated problem

> [!warning] Fidelity versus interpretability
> An extracted rule set, automaton or heatmap is a **second model** that
> approximates the first. If it matched exactly, the original would be
> redundant; since it does not, the explanation is to some degree false.
>
> How false is the **fidelity** question, and it is the central problem of the
> field [external]. The lecture never raises it, never measures it, and offers no
> method for checking an explanation is right ? which matters more here than the
> module's usual absence of numbers, because a confidently wrong explanation is
> worse than none.
>
> [[preference-moore-machine]]'s *"flexibility of abstraction level"* is the
> trade-off appearing as a parameter, unnamed. [[automata-extraction]] is the one
> method whose fidelity could be **measured** ? run the automaton and the network
> on the same inputs ? and the lecture does not suggest it.

## Why it belongs in a bio-inspired module

> [!note] The distributed representation, reconsidered
> L02 presented [[local-vs-distributed-representation|distributed coding]] as the
> brain's solution and the better engineering choice. L12 states the bill:
> *"distributed representations are difficult to understand and modify."*
>
> The brain is not interpretable either. So a module that has spent eleven
> lectures copying it should expect its artefacts to be opaque, and XAI is
> partly a demand that our systems be **more** legible than the thing they were
> modelled on. That is a coherent position and it sits in tension with
> [[ann-brain-correspondence]] ? the lecture does not notice the tension.

## See also

- [[symbolic-ai]] ? [[knowledge-extraction]] ?
  [[layer-wise-relevance-propagation]] ? [[class-activation-map]]
