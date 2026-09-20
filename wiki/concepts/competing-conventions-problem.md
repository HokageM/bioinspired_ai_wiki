---
title: Competing conventions problem
type: concept
sources: [L11]
tags: [evolution, neural-networks, representation]
updated: 2026-09-21
---

# The competing conventions problem

> **Competing Conventions Problem (same function in different way).**

Listed as a con of [[neuroevolution]], in six words, and it is the deepest
obstacle in the lecture.

## What it is

A neural network's hidden units have **no canonical order**. Permute them, permute
their weights to match, and you have a *different genome* computing an
*identical function*.

```
Network A hidden units:  [ edge-detector , colour-detector ]
Network B hidden units:  [ colour-detector , edge-detector ]

Same function. Completely different genome.

Crossover at the midpoint:
  child = [ edge-detector , edge-detector ]      # colour detection: gone
```

Both parents were competent. The child is broken, and nothing was wrong with the
crossover operator ? the *representation* was ambiguous.

## Why it is worse than it sounds

With `n` hidden units there are `n!` genomes for every function, so a population
of good solutions is almost certainly a population of **incompatible encodings of
similar behaviour**. [[recombination]] between two randomly chosen good parents
is then far more likely to duplicate one role and drop another than to combine
anything.

> [!success] This explains the lecture's own warning
> [[recombination]] says: *"Recombination can be destructive; favourable
> functions are not transferred to offspring, leading to low fitness. Has to be
> carefully designed, depending on topology and representation/encoding; may
> demand knowledge."*
>
> That is stated on page 7 as an empirical difficulty, and the *reason* for it
> appears on page 11 in six words, under a different heading, as a con of a
> different technique. They are the same problem. Joining them is the single most
> useful cross-reference in this lecture.

## Why it undermines the method, not just the operator

[[mutation]] says that without recombination *"evolution becomes a parallel
gradient search"*. Competing conventions is an argument that recombination on
networks is **usually** destructive. Put together: neuroevolution with a direct
encoding tends to degenerate into exactly that parallel gradient search ? the
population contributing cost without contributing the one thing that makes an EA
different in kind.

That is a serious charge against the method, assembled entirely from the
lecture's own statements, and not drawn by it.

## What would fix it

[external] The standard answer is **historical markings** ? track where each gene
originated so that crossover aligns genes by ancestry rather than by position
(the approach taken by NEAT). The source names neither the fix nor any method.
Per `CLAUDE.md` ?6 the wiki records the gap rather than filling it.

## See also

- [[neuroevolution]] ? [[recombination]] ? [[genetic-neural-encoding]]
