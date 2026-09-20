---
title: Memory replay and consolidation
type: concept
sources: [L13]
tags: [continual-learning, memory, neuroscience, plasticity]
updated: 2026-09-21
---

# Memory replay

> - **Memory reactivation and replay for memory consolidation**
> - **Cortico-hippocampal interaction** ? during sleep and awake phase
> - **Disrupting slow wave sleep impairs long-term memory consolidation**
> - **Learn also in the absence of sensory input**

The biological counterpart of the rehearsal strategy in
[[continual-learning-strategies]], and the biological core of
[[growing-dual-memory|GDM]].

## Why this passage matters more than it looks

> [!success] The module's first and only falsifiable experimental result
> *"Disrupting slow wave sleep impairs long-term memory consolidation"* is a
> **manipulation with a measured outcome**: intervene on sleep, memory gets worse.
> That is an experiment, and it could have come out the other way.
>
> Everything else in thirteen lectures is anatomy ("the LGN projects to V1"),
> description ("place cells fire at locations"), or assertion ("the brain is a
> hybrid system"). None of those could be wrong in the way this could.
>
> It arrives on page 2 of the final lecture, in one line, as support for a
> technique that did not need it.

## What replay is for

The hippocampus encodes an experience quickly; the cortex learns slowly and
generalises. Replay is the transfer: the hippocampus **reactivates** stored
patterns offline and the cortex trains on them, over and over, until the knowledge
is cortical [external]. This is [[complementary-learning-systems]].

**"Learn also in the absence of sensory input"** is the crucial property and the
whole point: the system can keep training on old tasks while they are *not
happening*. It is the biological answer to "no re-access to previously seen data"
? the data is regenerated internally.

```
while asleep or resting:
    pattern = hippocampus.reactivate()     # sampled from stored episodes
    cortex.train_on(pattern)               # slow, generalising updates
    # old tasks stay in the gradient without the world presenting them
```

Which is generative replay, with the hippocampus as the generator.

> [!note] Direction of inspiration, reversed
> Most bio-inspiration in this module runs *biology ? algorithm*:
> [[jeffress-model|delay lines]], [[receptive-field|receptive fields]],
> [[evolutionary-algorithm|evolution]]. Replay buffers were invented because
> shuffling data helps optimisers, and the neuroscience was attached afterwards.
>
> That does not make the parallel wrong ? it is one of the closest in the module ?
> but it is **convergence**, not inspiration, and it belongs in the
> [[ann-brain-correspondence]] scorecard under a heading the module has never
> used: *arrived at independently from both directions*, which is much stronger
> evidence than either alone.

## See also

- [[complementary-learning-systems]] ? [[continual-learning-strategies]] ?
  [[growing-dual-memory]] ? [[catastrophic-forgetting]] ? [[complementary-learning-systems]]
