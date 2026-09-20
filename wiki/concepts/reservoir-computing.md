---
title: Reservoir computing
type: concept
status: stub-by-source
sources: [L10]
tags: [recurrent, neural-networks, learning]
updated: 2026-09-21
---

# Reservoir computing

**Named once, in the final summary of [[L10-gesture-recognition]], and developed
nowhere:**

> **Reservoir computing models by neurophysiological principles for action
> selection in prefrontal cortex and striatum.**

Nothing in the twelve preceding pages mentions it. No architecture, no training
rule, no diagram, no citation.

## What the sentence does contain

Two claims worth recording even though the mechanism is missing:

**1. It is offered as an alternative to the trained recurrent networks of this
lecture.** The summary lists it immediately after *"RNN in LSTM and Gamma GWR"*,
so it belongs in the same slot ? a way of handling temporal structure.

**2. It is tied to specific anatomy for a specific function**: **prefrontal
cortex and striatum**, for **action selection**.

> [!warning] The striatum arrives without reinforcement learning
> The striatum is the module's **first basal-ganglia structure** and the
> canonical substrate of reward-based learning. [[learning-paradigms]] has had
> reinforcement learning defined and unused since L03. L10 names the anatomy
> associated with it, for the function it performs ? **action selection** ? and
> still does not name the algorithm.
>
> Action selection is also exactly what [[behaviour-coordination]] (L08) was
> about, resolved there by hand-set gains. The module has now approached the same
> problem from robotics, from attention ([[winner-take-all]]) and from anatomy,
> without joining them.

## Kept as a stub

Per `CLAUDE.md` ?6, the wiki does not supply what the source omits. [external]
Reservoir computing has a standard definition involving a fixed random recurrent
network with only the read-out trained; the module does not state it, so neither
does this page.

## See also

- [[gated-recurrent-network|LSTM]] ? [[gamma-gwr]] ? [[learning-paradigms]] ?
  [[behaviour-coordination]]
