---
title: Local vs Distributed Representation
type: concept
tags: [neural-networks, coding, representation]
sources: [L02, L04, L06, L07, L09, L12, L13]
created: 2026-09-20
updated: 2026-09-21
status: solid
---

# Local vs Distributed Representation

The lecture's answer to "how can we represent input vector encoding?" (L02, p3):
two schemes for mapping items onto neurons.

## Biological origin

The local scheme is named after the **grandmother cell** hypothesis — the idea
that a single neuron could stand for a single concept. The same one-unit-one-thing
structure appears in L01's [[place-cells]], where one cell is active in one
place.

## The two schemes (L02, p3)

### Local
**One neuron stands for one item** ("grandmother cell").

Drawn as a diagonal activation matrix over items A, B, C — exactly one unit on
per item.

| | Assessment (L02) |
|---|---|
| Problems | **Scalability problem** — $N$ items need $N$ neurons; **robustness problem** — lose the neuron, lose the concept |
| In practice | "Use this in practice with the **softmax function**" |

The practical note is the important one: a local code is a bad *internal*
representation but the normal *output* representation, where softmax turns the
one-hot target into a probability distribution over classes.

### Distributed
**Neurons encode features.** One neuron participates in more than one item, and
one item activates more than one neuron.

Drawn as a dense, overlapping activation matrix over items A, B, C.

| | Assessment (L02) |
|---|---|
| Advantages | **Better scalability**; **robust to damage or noise** |

Scalability improves combinatorially: $N$ binary feature units can distinguish
up to $2^N$ items rather than $N$.

## Computational form

```text
# LOCAL — one-hot. N items need N neurons.
encode_local(item, items):
    v = zeros(len(items))
    v[index_of(item)] = 1
    return v
# decode: item = argmax(v)          # in practice: softmax(v) for a distribution
# damage: kill unit k -> item k becomes unrepresentable. Total loss.

# DISTRIBUTED — feature code. N neurons can span up to 2^N items.
encode_distributed(item, features):
    return [1 if item.has(f) else 0 for f in features]
# decode: nearest-neighbour over the codebook (see
#         [[neural-similarity-and-dot-product]] — this is a dot product)
# damage: kill unit k -> every item loses one feature. Graceful degradation.
```

Graceful degradation is the whole argument: in a distributed code, no single
unit is critical to any single item, so noise and damage cost accuracy rather
than concepts.

## Where it appears in the module

- [[L02-spiking-neural-networks]] — page 3, closing the static-unit section.

## See also

- [[place-cells]] — a biological local code.
- [[neural-similarity-and-dot-product]] — decoding a distributed code is
  template matching.
- [[temporal-coding]] — scheme (b), coincidence coding, is a distributed code
  that lives in time rather than in activity level.
- [[activation-function]] — softmax is named here but not listed there.
- [[word-embedding]] (L04) — the same axis applied to words: one-hot codes are
  local, embeddings distributed.
- [[embodied-language-representation]] (L04) — the module's strongest
  distributed claim.

## L04 takes a side

L02 laid out local vs distributed as two options and did not choose. L04
chooses, at the level of whole faculties rather than single units:

| | Local | Distributed |
|---|---|---|
| Faculties | [[phrenology]] — one bump, one faculty | [[embodied-language-representation]] — *involves the whole brain* |
| Language areas | [[language-areas-of-the-brain]] — two boxes | [[dual-stream-hypothesis]] — overlapping pathways |
| Words | atomic one-hot vectors | [[word-embedding]] |

The evidence offered is the **"shark"** observation: the word activates visual
cortex, so its representation is not confined to a language area. And the
engineering payoff is the one this page already predicted — a distributed code
supports similarity, so `Queen = King + Woman − Man` works and one-hot
arithmetic cannot.

## L06: the place code, and a code built on subtraction

L06 adds two things to this axis.

**1. The ordered-population code appears again.** [[orientation-tuning]] gives
V1 cells a preferred edge angle with a graded falloff, so no single cell reports
orientation — the population does. That is the fifth instance of the same trick,
after [[place-cells]] (L01), [[self-organising-map]] units (L04),
[[tonotopic-representation]] (L05) and the [[jeffress-model]]'s coincidence
detectors (L05). See [[overview]]. It sits between the two poles of this page:
each unit is *locally* interpretable (it has a preferred value), yet the
quantity is only recoverable from the *distribution* of activity.

**2. A code defined by what it discards.** The centre-surround
[[receptive-field]] is the module's clearest example of a representation chosen
for what it throws away. Because excitation and inhibition cancel, uniform
illumination produces no response at all:

```
response = centre − surround      ⇒  uniform light ⇒ 0
```

The retina does not transmit brightness. It transmits *change*. The same
zero-sum structure recurs in L06's convolution kernel
`(-1 -1 -1 / 2 2 2 / -1 -1 -1)`, and in [[pooling]], which deliberately
destroys position information to buy translation invariance.

This is a third option alongside local and distributed: a representation is also
characterised by **which distinctions it refuses to make**. The module never
frames it this way, but L06 supplies three examples in one lecture.

## Open questions / gaps

- Softmax is named without being defined anywhere in the notes.
- The notes do not say how a distributed code is *learned*, only that it is
  better. [[hebbian-learning]] on page 7 is the module's only candidate so far.
- No mention of sparse coding as the middle ground between the two.
- The ordered-population code now has **five** instances across five lectures
  and has still never been named or discussed as a pattern.


## L07: a population code that is explicitly a probability (L07)

The [[histogram-based-som]] of [[L07-crossmodal-processing]] stores a
**histogram per input dimension** in each unit instead of a prototype vector, and
its output ? after **divisive normalisation** ? is described as a
**population-coded probability density function**.

This is the sharpest version of the distributed-code idea in the module. The
pattern of activity across the population is not merely *a* code for the input;
it is a **distribution over** what the input might be. The width of the pattern
carries the uncertainty.

That buys something no earlier code on this page could express: a unit can be
*unsure*. And uncertainty is exactly the quantity [[optimal-cue-integration]]
requires in order to weight modalities correctly ? which is why L07 needed the
upgrade.

| Code | Carries | Can express doubt |
|---|---|---|
| one-hot / grandmother cell | identity | no |
| [[word-embedding]] | similarity | no |
| ordered population ([[place-cells]], [[orientation-tuning]]) | a value | implicitly, via tuning width |
| **population-coded PDF (L07)** | **a distribution over values** | **yes, explicitly** |


## A priority code (L09)

The [[saliency-map]] is a population code whose activity means neither identity
nor value but **how much this location deserves attention**.

| Code | Activity means |
|---|---|
| one-hot | *which* |
| ordered population ([[place-cells]], [[orientation-tuning]]) | *what value* |
| population-coded PDF (L07) | *what value, and how sure* |
| **saliency map (L09)** | **how much this deserves processing** |

The last is a code about the *system's own resources* rather than about the
world — the only one in the module that is. And it is deliberately
**feature-blind**: it must be, because [[winner-take-all|one decision]] has to be
made from heterogeneous evidence, which requires a single comparable surface.
Same architectural need as L07's [[superior-colliculus|SC]] maps in register.

## L12 ? the same property, judged the opposite way

L12's neural-versus-symbolic comparison table lists representation as
**distributed** (neural) versus **localist** (symbolic), and then characterises
the two as:

> **compact but distributed** (neural) ? **verbose (leading to brittleness)**
> (symbolic)
>
> and, of neural representations: **"distributed representations are difficult to
> understand and modify"**.

> [!warning] This reverses the module's own earlier verdict
> | | Criterion | Verdict on distributed coding |
> |---|---|---|
> | **L02** | **robustness** ? graceful degradation, generalisation, no single point of failure | the **advantage** |
> | **L12** | **legibility** ? can a human read it, can a human edit it | the **cost** |
>
> Both are correct, and they are the same fact seen from two sides. The property
> that makes a distributed code survive damage ? no unit is individually
> necessary or individually meaningful ? is exactly the property that makes it
> unreadable. You cannot lesion a concept, and you cannot find one either.
>
> Neither lecture mentions the other, and L12 does not acknowledge that it is
> reversing a sign. This is the clearest instance in the module of a trade-off
> presented twice as two separate one-sided claims.

And symbolic coding gets the mirror treatment: *verbose leading to brittleness* ?
L12 concedes the localist cost that L02 named (one unit lost, one concept lost)
while claiming the legibility L02 ignored.

The honest statement the module never makes: **a representation cannot be both
maximally robust and maximally legible, because both properties are consequences
of how many units a concept occupies.** Everything in
[[neural-symbolic-integration]] is an attempt to get both by keeping two
representations at once.

## L13 — the third verdict, and the common root

L13 adds the last and most concrete cost of distributed coding:
**[[catastrophic-forgetting|it is the cause of catastrophic forgetting]]**.
Weights are shared across tasks, gradients refer only to the current task, so
learning anything new overwrites everything old.

| Lecture | Criterion | Verdict |
|---|---|---|
| **L02** | robustness | **advantage** — graceful degradation, generalisation |
| **L12** | legibility | **cost** — "difficult to understand and modify" |
| **L13** | retention | **cost** — "affects all connectionist models" |

> [!success] One fact, three consequences — stated here for the first time
> **A concept has no address.**
>
> - You cannot destroy it selectively → L02's robustness.
> - You cannot find and read it → L12's opacity.
> - You cannot protect it while changing its neighbours → L13's forgetting.
>
> Three lectures, three verdicts, one property. No lecture connects them, and the
> two costs are stated as universals while the benefit is stated as a feature.
>
> It also explains why the fixes rhyme: [[continual-learning-strategies|dynamic
> architectures]] and [[knowledge-extraction]] both work by **giving concepts
> addresses** — a dedicated column per task, a symbol per cluster. The retreat
> from full distribution is the same retreat in both lectures, for two different
> reasons.
