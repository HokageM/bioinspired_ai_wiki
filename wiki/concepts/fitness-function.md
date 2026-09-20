---
title: Fitness function
type: concept
sources: [L11]
tags: [evolution, optimisation, methods]
updated: 2026-09-21
---

# Fitness function

> - **Represents requirements for the adaptation**
> - **Basis for selection**
> - **Assigns quality measure to genotypes**
> - Synonyms: evaluation function, objective function
>
> **Example:** genotype `z` binary of `x`, phenotype `x ? ?` ? fitness
> `f(z) = 1/x?`.

## It is where the engineer states the problem

Everything else in an [[evolutionary-algorithm]] is problem-independent
machinery. The fitness function is the *only* place the actual problem enters,
which makes it the single most consequential design decision ? and the one the
lecture warns about most sharply:

> **Do not constrain the search space too much, e.g. through very detailed
> fitness functions or very specialised and goal-oriented operators.**
> **Let evolution do its job to find solutions.**
>
> **Good fitness function important. But: problems can be detected when
> observing best individual and average fitness of population.**

> [!note] The diagnostic is the best practical advice in the lecture
> Track **best** and **average** fitness together. If best rises while average
> stays flat, one lineage is running away and diversity is collapsing. If both
> plateau early, selection pressure is too high ? see
> [[premature-convergence]]. If neither moves, the fitness function is not
> discriminating.
>
> Two numbers, and they diagnose most of the failure modes on this page.

## The circularity that is not a problem here

[[four-pillars-of-evolution]] defines biological fitness by reproductive success,
which is circular. In an algorithm the fitness function is **stipulated in
advance** and is independent of how many offspring anything has.

That is the substantive difference between the metaphor and the method, and it is
also the method's main weakness: **you have to know what you want, numerically,
before you start.** In biology no one specifies the objective.

## Problems with the worked example

> [!warning] `f(z) = 1/x?`
> - It is written as a function of the **genotype** `z` but evaluated on the
>   **phenotype** `x`, with the decoding step unstated.
> - It is undefined at `x = 0` and diverges there, so the maximum of the stated
>   function is at the point where it does not exist.
> - Nothing says whether the goal is to maximise or minimise, though `1/x?`
>   implies *smaller x is better*.

## A real one

[[collision-free-navigation]] gives the module's only fitness function in closed
form for a real task:

`? = V ? (1 ? ??V) ? (1 ? i)`

where `V` = rotation speed, `?V` = difference of the wheel speeds, and `i` =
activation of the highest sensor. Three factors, multiplied: **go fast**, **go
straight**, **stay away from things**. Multiplication means any factor near zero
kills the score, so all three must be satisfied at once ? an AND, not a
compromise. The source gives the formula and does not analyse it.

## See also

- [[fitness-landscape]] ? [[parent-selection]] ? [[collision-free-navigation]] ?
  [[premature-convergence]]
