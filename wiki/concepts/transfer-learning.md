---
type: concept
title: Transfer Learning
sources: [L04, L13]
tags: [learning, generalisation]
---

# Transfer Learning

Reusing what was learned on one task to learn another faster or better.

## In L04

Named in the *epigenetic autonomous robot* section as the step from basic to
higher-order grounding:

- **Basic grounding** — the imitator learns to execute actions, learning
  **action and names simultaneously**.
- **Transfer: higher-order grounding** — the imitator learns **behaviours**:
  *combined actions*, with a *linguistic description in natural language*.
- ⇒ **Grounding of names and concepts of new actions.**

So the transfer here is **primitives → compositions**. Motor primitives and
their names, learned by imitation, become the vocabulary for behaviours that
were never demonstrated. The frozen lower layers supply the grounding; only the
composition is new.

It also appears for [[hubert]]: *the pre-trained model can be adapted to new
domains* — the familiar pretrain-then-finetune form.

## Two senses, one word

| Sense | Example in L04 | Transferred thing |
|---|---|---|
| **Compositional** | [[imitation-network]] | learned primitives, reused as parts |
| **Representational** | [[hubert]], [[gpt]] | learned features, reused as a starting point |

These are genuinely different mechanisms and the lecture uses the same term for
both. The first is closer to how the module talks about
[[compositionality-of-language]]; the second is ordinary deep-learning practice.

## Relation to the module

L03's [[learning-paradigms]] classified learning by *what supervision is
available*. Transfer learning cuts across that axis — it is about *what you
start from*, not how you are taught. A transferred model can then be fine-tuned
supervised, unsupervised or self-supervised.

Biologically, the compositional sense is the interesting one: it is the claim
that **linguistic abilities strictly depend on motor skills and behaviours**
(L04, p6). Learn to act, and language comes along for the ride.

## Unclear in the source

- No mechanism is given for *how* the transfer happens — whether layers are
  frozen, re-trained, or composed by a separate module.
- "Epigenetic" is not defined. See [[imitation-network]].

## See also

[[imitation-network]] · [[symbol-grounding]] ·
[[compositionality-of-language]] · [[self-supervised-learning]] ·
[[learning-paradigms]] · [[hubert]] · [[L04-embodied-language-processing]]

## L13 — multi-task transfer learning

Listed as the second of four paradigms related to
[[continual-learning|continual learning]]:

> - **Apply knowledge in one domain to a novel task**
> - **Positive and negative transfer**
> - **Forward and backward transfer**

Diagram: two tasks `TA` and `TB` with arrows in **both** directions.

Two independent distinctions, easily confused:

| | |
|---|---|
| **positive / negative** | does prior learning **help** or **hurt**? |
| **forward / backward** | does learning `A` affect `B`, or does learning `B` affect `A`? |

Crossing them gives four cases, and one of them is already familiar:
**negative backward transfer is [[catastrophic-forgetting|catastrophic
forgetting]]**. Forgetting is not a separate phenomenon but the worst corner of
transfer, which reframes the whole lecture — and L13 does not say it, despite
putting the two topics six pages apart.

The complementary corner matters for measurement: **positive backward transfer**
(an old task improves because you learned a new one) is why
[[continual-learning-metrics|the forgetting metric]] measures against `max_m
a_mj` rather than `a_jj`.
