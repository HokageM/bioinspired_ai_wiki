---
title: Hybrid integration architectures
type: concept
sources: [L12]
tags: [hybrid, architecture, representation]
updated: 2026-09-21
---

# Types of hybrid integration architecture

## The knowledge-technology stack

> a) **Symbolic knowledge and understanding**
> b) **Neural / statistical knowledge representation**
> c) **Sensory input from several modalities** (audio, vision, etc.)

```
        ????????????????????????????
   a)   ?   symbols, inference     ?   ?
        ????????????????????????????   ?
   b)   ?   neural representation  ?   ?  increasing abstraction
        ????????????????????????????   ?
   c)   ?   sensory input          ?   ?
        ????????????????????????????
```

The arrow runs **upward only**. Symbols sit on top of neural representations
which sit on sensors ? so this is a pipeline, and the same
[[visual-pathway|bottom-up]] picture the module has drawn since L06.

> [!note] No downward arrow, in a lecture that needs one
> L07, L08 and L09 all established [[top-down-modulation]]: higher levels **bias**
> lower ones. If symbols sit above neural representations, the obvious
> question is whether symbolic knowledge can constrain perception ? which is what
> *"go close to the table"* requires, and what makes neuro-symbolic systems
> interesting rather than merely readable.
>
> The next section supplies the vocabulary (*bidirectional*, *tight coupling*)
> and this diagram does not use it. Three lectures of top-down modulation, absent
> from the stack that most needs it.

## Transfer architectures

> **Hybrid transfer architecture: knowledge is transferred.**
> `NN` ? `Sym` : **symbolic representation of neural representation**

One-off extraction. The network is trained, the knowledge is pulled out, and the
result is a **description**. See [[weight-based-transfer]] and
[[automata-extraction]].

> **Hybrid transformation architecture:** symbolic representation of neural
> representation ? **automatic insertion or extraction of symbolic knowledge** ?
> **explanation of NN**.

*Insertion* is the reverse direction and the more interesting one: put known
rules **into** a network before training, so it does not have to learn what is
already known. The lecture names it in two words and never returns.

## Processing architectures

> **Hybrid processing architecture for hybrid realization of symbolic
> structures.**

Graded by coupling:

| | Diagram | Description |
|---|---|---|
| **Loose coupling** | `NN ? Symb` | **symbolic and neural modules separable and unidirectional communication** |
| **Tight coupling** | `NN ? Symb` | **symbolic and neural modules separable and bidirectional communication** |
| **Integration** | `(NN Symb) (NN Symb) (NN Symb)` | **symbolic and connectionist modules fully embedded and integrated** *(work together)* |

```
loose:        Symb ??????? NN                   one-way, still two systems
tight:        Symb ??????? NN                   two-way, still two systems
integrated:   [NN+Symb] [NN+Symb] [NN+Symb]     not two systems any more
```

> [!note] The axis is separability, and it runs from engineering to a research problem
> **Loose** and **tight** keep two systems and standardise the interface ? an
> integration problem, solvable today. **Integration** abandons the boundary:
> each module is *both*, so there is no interface to specify and no translation
> step to lose information at.
>
> The lecture gives no example of the third, and this is where the difficulty
> lives. Loose coupling is a pipeline; full integration requires a single
> representation that is simultaneously a vector and a symbol, and nothing in
> twelve lectures is one.

> [!note] Compare the module's other coupling taxonomies
> [[fusion-strategies]] (L07) graded *early / late / hybrid* fusion by **where**
> two streams join. This grades by **how tightly** two paradigms interpenetrate.
> Same shape of question ? one about data, one about formalism.

## See also

- [[neural-symbolic-integration]] ? [[weight-based-transfer]] ?
  [[knowledge-extraction]] ? [[fusion-strategies]]
