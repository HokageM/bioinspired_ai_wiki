---
title: Symbolic AI
type: concept
sources: [L12]
tags: [history, representation, foundations, explainability]
updated: 2026-09-21
---

# Symbolic AI

> `IF ? AND ? AND ? THEN ?`
>
> **Strong AI research in inference systems to model human recognition.**
> - Symbolic representations and their manipulation; **symbolic understanding**
> - Sequential list processing, recursion (**Prolog, Lisp**)
> - **Small domains only**

The tradition the rest of the module is not. Twelve lectures in, this is the
first mention of the other half of AI's history.

## What it is good at

**Composition.** A rule about *all birds* applies to a bird seen for the first
time, because the rule quantifies over a variable rather than interpolating
between examples.

**Inspection.** The knowledge is written where a person can read it, which is why
[[explainable-ai]] is framed in the lecture as a **return** to symbols.

**Recursion.** *"Sequential list processing, recursion"* ? a structure of
unbounded depth, from a finite description. No architecture in the module does
this; the closest is [[gated-recurrent-network|LSTM]], which iterates but does not
nest.

**Precision.** A rule either fires or it does not.

## What it is bad at

> **Small domains only.**

Three words for the failure that ended the tradition's dominance. The usual
diagnosis [external] is the **knowledge acquisition bottleneck** ? every rule must
be written by a person ? plus **brittleness**, which the lecture's own comparison
table names:

> Representation: **verbose (leading to brittleness)**

Brittleness is the direct cost of precision. An input matching no rule produces
nothing at all, where a network always produces *something* ? and degrades
gracefully, because a [[local-vs-distributed-representation|distributed code]]
has no cliff edge.

> [!note] The two traditions fail in exactly opposite ways
> | | Symbolic | Neural |
> |---|---|---|
> | Knowledge comes from | a person writing rules | data |
> | Scales with | **human effort** | **compute and data** |
> | Novel input | no rule fires: brittle | interpolates: graceful |
> | Novel *structure* | composes correctly | usually fails |
> | Can be read | yes | *"difficult to understand and modify"* |
>
> Neither column dominates, which is the argument for
> [[neural-symbolic-integration]] and the reason it has never been settled.

## "Cognition is not precise"

The lecture's rebuttal, with the module's best one-line example:

> **"Go close to the table."**

*Close* has no threshold. A symbolic planner needs a number and the concept does
not supply one; a neural controller can produce the motion and has no concept
that transfers to a chair. The instruction is **symbolic in form and continuous
in execution**, which is precisely the join the lecture argues for ? and the same
problem as [[embodied-language-representation|grounding]] (L04), arrived at from
the opposite direction.

## See also

- [[neural-symbolic-integration]] ? [[hybrid-integration-architectures]] ?
  [[explainable-ai]] ? [[local-vs-distributed-representation]]
