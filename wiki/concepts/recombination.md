---
title: Recombination (crossover)
type: concept
sources: [L11]
tags: [evolution, optimisation]
updated: 2026-09-21
---

# Recombination

> **Variation operator: create new individuals from parents.**
> Motivation: **diversity drives changes**. Synonym: **crossover operator**.
> **Distinguishes EA from other optimization algorithms.**
> **Aim: combine "good" parts of two parents to produce fitter offspring.**
> **Leads to diversity, but often a destructive jump in fitness landscape.**
> Crossover often applied with probability `p_c ? [0.5, 1.0]`.
> Implementation depends on representation form.

## The claim and the caveat, in consecutive lines

*Combine good parts of two parents* is the **building-block hope**: that a
solution decomposes into parts whose merit is independent, so that merging good
parts yields a better whole.

*Often a destructive jump* is the admission that this frequently fails. Merit
usually is **not** independent ? a weight is good only relative to the other
weights around it ? so the offspring lands somewhere neither parent's
neighbourhood covers, and usually lower. See [[fitness-landscape]].

> [!note] Recombination is the module's only non-local search move
> Everything else in eleven lectures takes **small steps**: gradient descent,
> SOM updates, GWR adaptation, mutation. Crossover jumps. That is precisely why
> it is *"what distinguishes EA from other optimization algorithms"*, and
> precisely why it is destructive. High variance is the price of the only
> operator that can leave a basin in one move.

## n-point crossover

> **Split parents at n points and recombine segments.**
>
> **Positional bias:**
> - n-point crossover **tends to keep together genes located close to each other**
> - **one-point can never keep together genes from opposite ends**
> - **knowledge on problem structure often not available**
>
> Example: `111|000` and `101|010` ? `110|010` and `101|000`

```
def n_point_crossover(a, b, n):
    points = sorted(random_sample(range(1, len(a)), n))
    child1, child2, swap = [], [], False
    for i in range(len(a)):
        if i in points: swap = not swap
        child1.append(b[i] if swap else a[i])
        child2.append(a[i] if swap else b[i])
    return child1, child2
```

The positional bias is **inherited from biology** ? chromosomes physically break
and rejoin at points along their length (see [[dna-and-heredity]]). It is
beneficial when genes that belong together sit together, and the third bullet is
the fatal qualification: *you usually do not know which genes belong together*.

> [!note] But sometimes you do
> [[genetic-neural-encoding]] gives two layouts for the same network, and in one
> of them a node and its incoming weights are contiguous. Choosing that layout
> makes the positional bias work **for** you. The lecture supplies both halves of
> this argument, two pages apart, and does not join them.

## Arithmetic recombination

> **Use function to combine values.** Powerful for float-point representations;
> **produces new genes**.
>
> `0.1 0.3 0.5 | 0.7 0.9` and `0.5 0.9 0.7 | 0.3 0.1`
> ? `0.1 0.3 0.5 | 0.5 0.5` and `0.5 0.9 0.7 | 0.5 0.5`

```
def arithmetic_recombination(a, b, lo, hi, alpha=0.5):
    child = list(a)
    for i in range(lo, hi):
        child[i] = alpha*a[i] + (1-alpha)*b[i]      # the average, here
    return child
```

> [!note] "Produces new genes" is the key phrase, and it changes the operator's character
> n-point and uniform crossover only **redistribute** alleles that already exist
> in the population. If every parent has `0` at position 3, no amount of
> crossover will ever produce a `1` there ? only [[mutation]] can.
>
> Arithmetic recombination **creates values neither parent had**, so it is a
> variation operator with mutation-like reach, available only on continuous
> representations. It is also *contractive*: averaging pulls offspring towards
> the population mean, so used alone it destroys diversity.

## Uniform crossover

> **Swap based on vector of random values: genes are distributed among
> children.** Marginal note: *"n-point crossover?"*

```
def uniform_crossover(a, b):
    mask = [random() < 0.5 for _ in a]      # independent per gene
    return [a[i] if m else b[i] for i, m in enumerate(mask)]
```

**No positional bias** ? every gene is decided independently, so distance along
the genome is irrelevant. The right choice when you have no reason to believe
neighbouring genes belong together, which the lecture has just said is the usual
case.

It also has **no biological original**: no chromosomal mechanism shuffles genes
independently. Pure engineering, listed beside the biologically-derived operator
without the distinction being drawn.

## Recombining two neural networks

> **Recombination can be destructive; favourable functions are not transferred to
> offspring, leading to low fitness.**
> **Has to be carefully designed, depending on topology and representation /
> encoding; may demand knowledge.**

The honest verdict, and the reason is [[competing-conventions-problem]]: two
networks can compute the same function with permuted hidden units, so combining
them yields a network that does neither job.

## See also

- [[mutation]] ? [[candidate-representation]] ? [[fitness-landscape]] ?
  [[competing-conventions-problem]]
