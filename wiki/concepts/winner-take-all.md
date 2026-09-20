---
title: Winner-take-all
type: concept
sources: [L09, L11]
tags: [attention, competitive-learning, coding]
updated: 2026-09-21
---

# Winner-take-all

> **Winner-take-all is the core algorithm** (of a saliency model).
>
> **Only the most salient or active one is selected.**
> *A feature or region in an image is identified; ignore the others.*

```
winner = argmax(activity)
```

That is the whole rule. Its importance is that it converts a **graded map** into
a **discrete choice**, which is what any system with one output and many
candidates eventually needs.

## The module's fifth encounter with it

| Lecture | Where | What competes | What the winner gets |
|---|---|---|---|
| L04 | [[self-organising-map]] | prototype units | the weight update |
| L07 | [[fusion-strategies]] | modalities | the output |
| L08 | action selection / voting | behaviours | control of the actuators |
| L08 | [[subsumption-architecture]] | layers | the motor signal (by suppression) |
| **L09** | **[[saliency-map]]**, **[[auditory-attention-model|object competition]]** | **locations / auditory objects** | **attention** |

Five lectures, five vocabularies ? *best matching unit*, *max fusion*, *action
selection*, *subsumption*, *winner-take-all* ? one operation. L09 is the only
one that names it as an algorithm in its own right, and it does not connect it
to the others.

> [!note] What makes it biologically comfortable
> WTA is implementable with **local lateral inhibition**: every unit suppresses
> its neighbours in proportion to its own activity, and the network settles with
> one survivor. No comparator, no global `argmax`, no read-out of all values to
> one place. That is why the same primitive keeps reappearing ? it is one of the
> few decision rules a sheet of neurons can actually perform. See
> [[excitatory-and-inhibitory-neurons]].

```
# argmax, the way cortex could do it
def wta(a, k=0.2, steps=50):
    for _ in range(steps):
        inhibition = k * (sum(a) - a)      # each unit inhibited by all others
        a = relu(a - inhibition)           # local, parallel, no comparator
    return a                               # one unit survives
```

## The cost

WTA discards everything except the winner, so the system becomes **serial**: to
attend to the second-most-salient location you must first **inhibit** the winner
and re-run. That is the *inhibition of return* step in the
[[saliency-model]] loop, and it is why visual search takes time proportional to
the number of candidates when the target does not
[[pop-out-effect|pop out]].

It is also the precise point where biological attention and
[[attention|transformer attention]] diverge: a softmax keeps all the losers,
weighted. WTA throws them away ? because the brain's constraint is capacity, and
the transformer has no such constraint.

## See also

- [[saliency-map]] ? [[saliency-model]] ? [[behaviour-coordination]] ?
  [[self-organising-map]]


## L11 ? argmax over a random subset

[[tournament-selection]] is the ninth instance, and the first with a twist:

> **Randomly select `k` individuals into a group `G` of contestants. Individual
> `i` with `f(i) = max_{i?G} f(i)` wins the tournament.**

| # | Lecture | Competitors | Prize |
|---|---|---|---|
| 1 | L04 | SOM units | be the BMU |
| 2 | L07 | modality estimates | max fusion |
| 3 | L08 | behaviours | motor control |
| 4 | L08 | subsumption layers | override |
| 5?6 | L09 | locations; auditory objects | salience; attention |
| 7 | L10 | CLIP class sentences | the label |
| 8 | L10 | GWR nodes | be the BMU |
| **9** | **L11** | **`k` random individuals** | **become a parent** |

> [!note] Randomising the competitor set turns a greedy rule into a tunable one
> Instances 1?8 all run argmax over a **fixed, complete** set: the winner is
> determined, and the rule is maximally greedy. A tournament runs argmax over a
> **random sample of size `k`**, and `k` then controls how greedy the outcome is
> ? from uniform random at `k = 1` to fully greedy at `k = ?`.
>
> That is a general trick the module never states: **subsample the competition to
> soften the competition.** It applies to every earlier instance. A saliency map
> read out by a tournament rather than a global max would sometimes attend to the
> second-most-salient location ? which is, in effect, what an exploration bonus
> does.
>
> The connection to [[selection-pressure]] is exact: `k` *is* the greediness of
> the argmax.
