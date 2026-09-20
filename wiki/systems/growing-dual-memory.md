---
title: Growing Dual-Memory network (GDM)
type: system
sources: [L13]
tags: [continual-learning, memory, architecture, vision, self-organisation]
updated: 2026-09-21
---

# Growing Dual-Memory networks (GDM)

The lecture's flagship system and the module's most complete architecture.

```
                                            ????????????????????
  G-SM   Semantic Memory  (category level)  ? spatiotemporal   ?
    ?                                       ? (context)        ?
  G-EM   Episodic Memory  (instance level) ?? learning with    ?
    ?                                       ? recurrent GWR    ?
  Convolutional Feature Extraction ?????????? "temporal        ?
    ?                                       ?  synapses" (t?1) ?
  Input image                               ????????????????????
```

Three levels, each a different kind of learner:

| Level | Type | Learns | Trained |
|---|---|---|---|
| Convolutional feature extraction | [[convolutional-network|CNN]] | generic visual features | offline / pretrained |
| **G-EM** ? episodic memory | [[gwr-network|GWR]] | **individual instances** | continually |
| **G-SM** ? semantic memory | [[gwr-network|GWR]] | **categories** | continually |

Plus **temporal synapses** ? recurrent `t?1` connections giving spatiotemporal
context learning, i.e. a recurrent GWR ([[gamma-gwr]]).

## Why it is built this way

It is [[complementary-learning-systems|complementary learning systems theory]]
turned into an architecture. Episodic memory below semantic memory, instances
below categories, with [[memory-replay|replay]] moving knowledge upward:

| CLS | GDM |
|---|---|
| hippocampus ? fast, instance-level, sparse | **G-EM** |
| neocortex ? slow, category-level, overlapping | **G-SM** |
| replay consolidates hippocampal ? cortical | replay trains G-SM from G-EM |

It is also a **hybrid** in [[continual-learning-strategies|the strategy taxonomy]]
? dynamic architecture (both memories are growing GWRs) *plus* memory replay ?
which is exactly the family the lecture recommends.

## Why GWR twice rather than one big one

The two memories need different **granularities**, not different algorithms. A
GWR's granularity is set by its growth thresholds `a_th`, `h_th`: strict
thresholds give a neuron per instance, loose ones give a neuron per category.
So the *same* algorithm with *different constants* yields episodic and semantic
memory.

> [!note] This is elegant and the lecture does not point it out
> The [[stability-plasticity-dilemma|stability?plasticity]] setting is a
> **parameter**, and a dual-memory system is two instances of one learner at
> opposite ends of it. That is a considerably better argument for CLS as an
> engineering principle than the biological analogy, and it is invisible in the
> notes ? you only see it once [[gwr-network|GWR's algorithm]] is written out,
> which happens on the next page.

## Problems

> [!warning] Every arrow in the diagram points up
> Input ? CNN ? G-EM ? G-SM. But **replay is downward** by construction: semantic
> memory must regenerate episodic patterns for consolidation to mean anything.
> The one mechanism the architecture exists for is the one the diagram omits.
>
> The same feed-forward-only drawing problem appears in
> [[top-down-modulation]] for L12 and contradicts L07 and L09 outright.

> [!warning] Unresolved: what does G-SM actually learn from?
> G-EM's activations, or replayed samples? The two give different systems, and
> the lecture does not say. This is the central design question.

The **CNN is not continual.** It is pretrained and frozen, so GDM's continual
learning happens entirely in the top two layers over fixed features. Genuinely
novel visual primitives cannot be learned ? a real limitation, unstated.

## See also

- [[gwr-network]] ? [[gamma-gwr]] ? [[complementary-learning-systems]] ?
  [[memory-replay]] ? [[continual-learning-strategies]] ? [[L13-continual-learning]]
