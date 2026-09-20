---
title: Evolutionary algorithm (the general scheme)
type: system
sources: [L11]
tags: [evolution, optimisation, architecture]
updated: 2026-09-21
---

# The general scheme of an evolutionary algorithm

```
   Initialisation ??? Population ??? Parent selection ??? Parents
                          ?                                  ?
                          ?                                  ? Recombination
       Survivor selection ?                                  ? Mutation
                          ?                                  ?
                          ??????????????? Offspring ??????????
                          ?
                          ?
                     Termination
```

## The terminology mapping

| Nature | Optimisation |
|---|---|
| Environment | **Problem** |
| Individual | **Candidate solution** |
| Fitness | **Quality** |

Three lines, and they are the whole translation. Once made, the biology is
dispensable ? see [[four-pillars-of-evolution]].

## Pseudocode

```
def evolutionary_algorithm():
    P = initialise(mu)                    # random, or heuristic
    evaluate(P)
    while not terminate(P):
        parents   = select_parents(P, mu)        # probabilistic, fitness-biased
        offspring = []
        for a, b in pairs(parents):
            if random() < p_c:                   # p_c in [0.5, 1.0]
                c = recombine(a, b)
            else:
                c = copy(a)
            c = mutate(c, p_m)
            offspring.append(c)
        evaluate(offspring)
        P = select_survivors(P, offspring, mu)   # mu + lambda  -->  mu
    return best(P)
```

## The two kinds of operator

> **Variation operators (recombination and mutation) depend on candidate
> representation.**
> **Selection operators only require fitness value.**

> [!note] A clean separation of concerns, and the reason the method is general
> Selection is **problem-independent**: give it numbers and it works, whatever
> they measure. Variation is **entirely problem-dependent**: you cannot mutate a
> tree the way you mutate a bit string.
>
> So porting an EA to a new domain means writing two functions ?
> `recombine` and `mutate` ? plus a [[fitness-function]]. Everything else is
> reusable. That, and not the biological metaphor, is why the method spread.

## The four dialects

> **Example representation of candidate solutions:**
>
> | Dialect | Representation |
> |---|---|
> | Genetic Algorithms | string over a finite alphabet, e.g. `01110` |
> | Evolution Strategies | real-valued vectors, e.g. `0.1` |
> | Evolutionary Programming | application-specific, e.g. FSM |
> | Genetic Programming | **trees** |

They differ *only* in representation, which is the point: one scheme, four
historically separate research traditions, distinguished by the data structure
they evolve. Genetic Programming is the outlier ? a tree is a **program**, so it
evolves code rather than parameters.

> [!warning] No attribution
> Four dialects, four originators, none named. Nor is it stated that these
> developed independently and were only later unified under this scheme.

## What an EA requires, and does not

**Requires:** a representation, a way to vary it, and a fitness value.

**Does not require:** a derivative, differentiability, continuity, convexity, or
even a numeric input space.

> [!note] The first gradient-free optimiser in the module
> [[backpropagation]] needs derivatives; [[self-organising-map|SOM]] and
> [[gwr-network|GWR]] need a metric space. An EA needs only that solutions can be
> **ordered**. That is why it can optimise a network **topology** ? a discrete
> object no gradient can reach ? which is precisely the problem
> [[neuroevolution]] exists to solve.

## See also

- [[parent-selection]] ? [[survivor-selection]] ? [[recombination]] ?
  [[mutation]] ? [[fitness-function]] ? [[candidate-representation]]
