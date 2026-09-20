---
title: Complementary learning systems
type: concept
sources: [L13]
tags: [memory, neuroscience, continual-learning, architecture]
updated: 2026-09-21
---

# Complementary learning systems theory

Named in L13's summary as one of the two biological bases of continual learning,
alongside [[hebbian-learning]]:

> **Bio: Hebbian learning & Complementary Systems Theory**

> [!warning] Named, never defined
> The theory is cited once, in a summary bullet, with no statement of what it
> claims. Everything below is reconstructed from what the lecture *does*
> elsewhere ? the [[growing-dual-memory|GDM]] architecture and the
> [[memory-replay]] passage are both direct expressions of it ? plus [external]
> background. `status: stub-by-source`.

## The claim

Two memory systems with **opposite** stability/plasticity settings, because no
single system can have both [external]:

| | Fast system | Slow system |
|---|---|---|
| Substrate | hippocampus | neocortex |
| Learning rate | high ? one exposure | low ? many exposures |
| Representation | **sparse, pattern-separated** ? episodes kept apart | **overlapping, distributed** ? structure shared across episodes |
| Stores | **instances** | **categories / statistics** |
| Forgetting | fast | slow |

The hippocampus can learn in one shot precisely *because* its representations
barely overlap ? which is L13's [[catastrophic-forgetting|forgetting]] analysis
run in reverse. **Interference is the price of overlap, so avoid overlap.** The
cortex accepts overlap, and therefore must learn slowly and needs
[[memory-replay|replay]] to do it.

## Why it is the answer to the dilemma

[[stability-plasticity-dilemma|Stability and plasticity]] are incompatible **in
one system**. CLS's move is to stop trying: build two systems at the extremes and
connect them. Plasticity lives in the hippocampus, stability in the cortex, and
replay moves knowledge from one to the other.

That is exactly [[growing-dual-memory|GDM]]'s G-EM (episodic, instance level) and
G-SM (semantic, category level).

> [!note] The module's oldest structural pattern, one last time
> Two subsystems with complementary properties and a transfer between them:
> [[two-visual-streams|dorsal and ventral]] (L06),
> fast and contextual pathways (L07),
> [[subsumption-architecture|layered behaviours]] (L08), stimulus- and goal-driven
> [[attention]] (L09), [[neural-symbolic-integration|neural and symbolic]] (L12),
> and now episodic and semantic memory. Six lectures, one architectural instinct,
> never named as such by the module.

## See also

- [[memory-replay]] ? [[growing-dual-memory]] ? [[stability-plasticity-dilemma]] ?
  [[memory-replay]] ? [[hybrid-architecture]]
