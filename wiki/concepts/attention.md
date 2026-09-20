---
title: Attention
type: concept
sources: [L09]
tags: [attention, neuroscience, foundations]
updated: 2026-09-21
---

# Attention

> **Cognitive control: multiple processes that plan and coordinate actions to
> meet task goals.**
> **Attention: the most important subfunction of cognitive control.**
> **Selective attention: like a filter with the ability to remove irrelevant
> information and thus optimise the current goal.**

The organising idea is **limitation**. There is more input than can be
processed, so something must choose. Attention is that choice, and the lecture's
epigraph is the whole argument in one line: *the art of being wise is the
ability to know what to overlook.*

## The four terms

| Term | Definition (source) | Question it answers |
|---|---|---|
| **Selective** | control awareness of the internal mind and the outside world; **integrate multidimensional and multimodal information** | *what* is selected |
| **Sustained** | maintain a state over time to detect the incoming stimulus; **enhance relevant stimuli and inhibit irrelevant distractors** | *for how long* |
| **Exogenous** | **bottom-up, stimulus-driven**; instinctive, spontaneous | *who initiated it* ? the world |
| **Endogenous** | **top-down, goal-driven**; spotlight, allocate limited (resources) | *who initiated it* ? you |

> [!note] These are three questions, not one taxonomy
> Selective/sustained is *what vs how long*; exogenous/endogenous is *who
> started it*. They cross: attention can be selective and exogenous, or
> selective and endogenous. The notes present the four as a flat list.

Two mechanisms recur in the definitions and are worth separating:
**enhance the relevant** and **inhibit the irrelevant**. A filter needs only the
second; a spotlight needs only the first. The lecture's definitions use both
without saying whether they are one mechanism or two.

## Two unrelated things called attention

> [!warning] The module uses "attention" in two incompatible senses
> [[gpt]] (L04) is described as *attention instead of recurrence*, and that is
> the entire explanation given. L09 defines attention at length and **never
> mentions transformers**.
>
> | | Psychological attention (L09) | Transformer attention (L04) |
> |---|---|---|
> | Motivated by | **limited capacity** | none ? it is not capacity-limited |
> | Selects | one location / stream; the rest is discarded | nothing; it **weights everything** |
> | Output | a spotlight, [[winner-take-all|one winner]] | a weighted average over all positions |
> | Top-down bias | an explicit goal signal | the query vector |
> | Serial | yes ? attend, inhibit, move on | no ? fully parallel |
>
> The single genuine point of contact is the last row of the table: a query
> against keys **is** a top-down bias being applied to a set of candidates, which
> is what [[exogenous-and-endogenous-attention|endogenous attention]] does. The
> difference is that the brain then *throws the losers away* and the transformer
> does not. Capacity limitation is the whole point biologically and is absent
> computationally.
>
> Recorded here because the module invites the confusion by using one word, and
> resolves it nowhere.

## Where attention has already appeared

- [[top-down-modulation]] (L07): cortex biasing [[superior-colliculus|SC]]
  fusion is endogenous attention under another name.
- [[object-picking-architecture]] (L08): *goal intention encoding* injected into
  visual processing ? likewise.
- [[reactive-agent]] (L08): a purely reactive agent is **exogenous only**. It
  cannot have endogenous attention, because it has no goal representation.

That last correspondence is exact and the module does not draw it: the
bottom-up/top-down dichotomy of L09 is the reactive/deliberative dichotomy of
L08.

## See also

- [[exogenous-and-endogenous-attention]] ? [[attention-networks]] ?
  [[saliency-map]] ? [[auditory-scene-analysis]] ? [[social-attention]]
- [[L09-bio-inspired-attention]]
