---
type: system
title: GPT
sources: [L04, L09, L12, L13]
tags: [nlp, transformer, generative, self-supervised]
status: stub-by-source
---

# GPT

L04's example of **natural language generation**.

> [!info] Thinly covered
> The lecture names GPT in nine bullet points and never defines attention, the
> transformer block, or the training objective in any detail. This page records
> exactly what the notes contain and marks the rest as absent.

## What the notes say

- **Based on the Transformer architecture.**
  - Only the **simplified decoder part** is retained.
  - Blocks are **stacked on top of each other**.
- **Attention instead of recurrence.**
- **Generative pre-training:**
  - guess the **next** word, given context
  - **unsupervised**
  - **auto-regressive**
- **Enormous amount of data.**

The margin diagram shows a stack with **self-attention** at the bottom, fed by
*text & position embedded*, with two heads on top: **text prediction** and
**task classifier**.

## Pseudocode

The notes do not contain an algorithm. This is the wiki's reconstruction of the
generative pre-training loop *as described in the bullets above*; the internals
of `transformer_block` are `[external]`.

```
# ---- generative pre-training (what the notes describe) ----
# corpus : long stream of tokens
# k      : context length

for each window w[1..k] sampled from corpus:
    h <- token_embed(w) + position_embed(1..k)
    for each of the N stacked decoder blocks:
        h <- transformer_block(h)        # causal self-attention + feed-forward
    p <- softmax(unembed(h))             # p[i] = distribution over next token
    loss <- - sum_i log p[i][ w[i+1] ]   # predict the NEXT token at every position
    update parameters by gradient descent on loss

# ---- auto-regressive generation ----
context <- prompt
repeat:
    next <- sample from model(context)
    context <- context + next
```

"Causal" — each position may attend only to positions before it — is what makes
one forward pass yield `k` training signals at once. The notes do not mention
this, but it is the whole reason the scheme is efficient.

## Why it matters to this module

It is the **end point of the L03 sequence thread**. L03 built
[[recurrent-neural-network]] → [[simple-recurrent-network]] →
[[gated-recurrent-network]] specifically to handle sequences, with the
[[vanishing-gradient-problem]] as the driving limitation. GPT's *attention
instead of recurrence* removes the recurrence entirely — and with it the
vanishing gradient.

## The tension

This is the least biologically motivated model in the module so far. It has:

- no local learning rule — global [[backpropagation]] through a deep stack;
- no embodiment — trained on text alone, which is precisely what
  [[embodied-language-representation]] argues is insufficient;
- no correspondence to any brain area.

L04 spends seven pages arguing that language is *embodied, distributed, grounded
in sensorimotor experience*, and then presents, without comment, a model that is
none of those things and works better. The lecture does not resolve this. See
[[ann-brain-correspondence]] and [[symbol-grounding]].

## Absent from the source

- Self-attention (queries/keys/values) — **never defined**.
- Multi-head attention, layer norm, residual connections — not mentioned.
- Model size, data size, which GPT version — not given.
- The "task classifier" head in the diagram is never explained.

## See also

[[hubert]] · [[word2vec]] · [[self-supervised-learning]] ·
[[gated-recurrent-network]] · [[vanishing-gradient-problem]] ·
[[ann-brain-correspondence]] · [[L04-embodied-language-processing]]


## The other attention (L09)

> [!success] The thread *"attention is used but never defined"* closes here —
> but not for this page
> [[L09-bio-inspired-attention]] defines attention thoroughly: selection under
> capacity limits, exogenous vs endogenous, three networks, saliency,
> winner-take-all. **None of it is transformer attention**, and L09 never
> mentions transformers.

What this page calls *attention instead of recurrence* differs from the
psychological construct on every axis that matters:

| | L09's attention | Self-attention here |
|---|---|---|
| Motivated by | **limited capacity** | parallelism |
| Effect on losers | discarded ([[winner-take-all]]) | **kept, down-weighted** (softmax) |
| Serial | yes — attend, inhibit, repeat | no |
| Top-down signal | an explicit goal | the query vector |

The one real correspondence is the last row: a query scoring a set of keys **is**
a top-down bias applied to candidates, which is what
[[exogenous-and-endogenous-attention|endogenous attention]] does. What is absent
is the bottleneck — and the bottleneck is the entire biological motivation.

So the module's use of one word for both is a **pun**, not a correspondence. See
[[attention]] and [[ann-brain-correspondence]].

## L12 ? ChatGPT as an explanation generator

L12 returns to ChatGPT in the [[explainable-ai|explainability]] section, in a role
it was not given in L03: not a language model, but a **tool for explaining other
models**.

> **General purpose LLM; prompt.**
>
> **Strengths in explanation generation**
> - (+) natural language
> - (+) versatile in explaining various ML tasks as it is a general-purpose model
> - (+) personalization
> - (+) interactivity
> - (+) multilingual
>
> **Weaknesses**
> - (?) hallucination, bias
> - (?) bad at reasoning, quality relies on data set
> - (?) hard to ensure the trustworthiness
> - (?) inconsistency
> - (?) redundant

> [!warning] This is the third and weakest sense of "explanation" in one lecture
> L12 uses the word for three different things without distinguishing them:
>
> | Method | What it produces | Grounded in |
> |---|---|---|
> | [[automata-extraction]], [[knowledge-extraction]] | rules / a state machine | the network's **weights and activations** |
> | [[layer-wise-relevance-propagation]], [[class-activation-map]] | a heatmap | the network's **forward computation** |
> | ChatGPT | fluent prose | **its training corpus** |
>
> The first two are derived *from the model being explained*. An LLM asked to
> explain a network has no access to that network at all ? it produces text that
> resembles explanations of similar networks. The listed weaknesses concede this
> ("hallucination", "hard to ensure the trustworthiness"), but the format puts it
> in the same list as the others as though it were a comparable option.
>
> Note also that the strengths are all properties of the **output medium**
> (natural, multilingual, interactive) while the weaknesses are all properties of
> the **content** (wrong, inconsistent, untrustworthy). That asymmetry is the
> finding, and the lecture does not draw it.

The entry is undated in the notes but plainly recent, and it is the only point in
the module where a contemporary commercial system is evaluated with a
strengths/weaknesses list rather than described.

## L13 — the recipe criticised

L13 names **transformer-based language models** as state of the art, gives
[[bert]] as the example, and then states the problem with the paradigm GPT
belongs to:

> **Knowledge about all tasks must be present prior to designing the model.**

Pretrain-then-fine-tune requires the task set in advance, which is the **batch
assumption** ([[continual-learning]]) inside the dominant method in NLP.

The proposed fixes — *view all tasks as question answering*, and *feed domain and
task information as input rather than encoding them in the architecture* — are,
unnamed, a description of **prompting**. L12 discussed ChatGPT prompting at
length without noticing that L13 explains why it exists: moving the task
specification from the weights into the input is what makes one model serve an
open-ended task set. See [[continual-language-learning]].
