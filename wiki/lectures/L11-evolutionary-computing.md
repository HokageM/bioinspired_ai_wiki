---
title: "L11 ? Evolutionary Computing"
type: lecture
lecture: 11
sources: [L11]
tags: [evolution, optimisation, neural-networks, robotics]
updated: 2026-09-21
---

# L11 ? Evolutionary Computing

**Source:** `raw/lectures/BioinspiredAIWdh11.pdf`, 11 pages, dated 16.02.2024.

> [!success] The module's longest-standing gap closes
> ~~Evolution and swarm intelligence are entirely absent from a module called
> **Bio-Inspired AI**~~ ? flagged in the wiki since L01 and repeated at every
> ingest through L10. L11 is evolution, in full: the biology, the algorithm
> scheme, every operator, and two applications.
>
> Ten lectures of **neural** inspiration, then one of **evolutionary**
> inspiration. Swarm and collective intelligence remain absent with two lectures
> to go.

## The argument that opens the lecture

> **Powerful problem solvers in nature ? Brain: "wheel" ? evolutionary
> mechanism, that created the human brain.**

This is the strongest framing move in the module, and it is made in three lines.
Everything to date has copied **the brain**; the brain is itself the *output* of a
search process; therefore copy **the search process**. It is bio-inspiration one
level up ? imitating the designer rather than the design.

The motivation is then narrowed to a concrete engineering complaint:

> **Artificial NN good candidate for problem solving and controllers of
> autonomous robots** ? ability to learn and adapt to dynamic environments,
> tolerant to noise, robustness, can represent complex functions through
> recurrent/lateral connections and non-linear transfer functions.
>
> **But: how to select necessary topology, weights and efficient learning
> parameters?**
>
> **Goal:** approximate problem solving based on the theory of evolution;
> integrate evolutionary algorithms and neural networks.

> [!note] Ten lectures of unanswered hyperparameters, collected into one question
> How many layers? How many units? What learning rate? Which activation? The
> module has chosen these silently since L03 and never once said how. L11 names
> the omission and proposes to **search** for the answers.
>
> It is also the second answer to the same problem in two lectures.
> [[gwr-network|GWR]] (L10) sizes a network from the data *during* learning; L11
> searches over sizes *across* a population. Adaptive structure, twice, by
> completely different means ? and the lectures do not reference each other.

## The four pillars of evolution

> All species derive from common ancestors.
>
> - **Population:** group of several individuals
> - **Diversity:** individuals have different characteristics
> - **Heredity:** characteristics are transmitted over generations
> - **Selection:** individuals produce more offspring than the environment can
>   support. Better at food gathering = better at surviving = make more offspring

These four are exactly the sufficient conditions for evolution to occur, and
they are also exactly the components of [[evolutionary-algorithm|the algorithm]].
That correspondence is the whole of evolutionary computing, and the source lets
it stand without comment ? see [[four-pillars-of-evolution]].

## The biology

[[genotype-and-phenotype]] ? the distinction that makes the whole method work:
**selection operates on the phenotype, variation operates on the genotype.**

[[dna-and-heredity]] ? nucleotides, genes, mitosis and meiosis, crossing-over and
mutation. The biological originals of `crossover` and `mutate`.

## The algorithm

[[evolutionary-algorithm]] gives the scheme, the terminology mapping
(*environment ? problem, individual ? candidate solution, fitness ? quality*) and
the four historical dialects. The operators each have their own page:

| Stage | Page |
|---|---|
| Representation | [[candidate-representation]], [[genetic-neural-encoding]] |
| Evaluation | [[fitness-function]], [[fitness-landscape]] |
| Parent selection | [[parent-selection]], [[fitness-proportional-selection]], [[ranking-selection]], [[roulette-wheel-selection]], [[tournament-selection]] |
| Variation | [[recombination]], [[mutation]] |
| Survivor selection | [[survivor-selection]] |

## Applications

[[neuroevolution]] ? evolving the weights, topology and hyperparameters of a
network, with the [[competing-conventions-problem]] as its characteristic
failure.

[[collision-free-navigation]] ? a robot controller evolved with the fitness
function `? = V(1 ? ??V)(1 ? i)`, which is the module's only
**hand-designed objective function** given in closed form.

[[genetic-inverse-kinematics]] ? a genetic algorithm solving
[[inverse-kinematics]] for a robot arm, with an honest pros/cons table.

## What is new here, structurally

**Search without gradients.** Every optimisation in the module so far has been
[[backpropagation|gradient descent]] or [[self-organising-map|competitive
learning]]. An EA needs only a **fitness value** ? no derivative, no
differentiability, no continuity. That is why it can optimise a *topology*, which
is a discrete object that no gradient can reach.

**Population instead of point.** Ten lectures optimise **one** parameter vector.
An EA maintains many and lets them compete, which is what buys escape from local
optima. It is also, quietly, the module's first **parallel** algorithm.

**Explore versus exploit, named at last.** Selection pressure exploits; mutation
and weak-parent selection explore. [[selection-pressure]] and
[[premature-convergence]] are the two poles, and the lecture is honest that every
operator choice trades one against the other.

> [!note] A fifth architectural axis
> | Lecture | Axis |
> |---|---|
> | L05 | learned ? specified |
> | L07 | fast ? contextual |
> | L08 | modelless ? modelled |
> | L09 | stimulus-driven ? goal-driven |
> | **L11** | **exploration ? exploitation** |

## Verified from the source

- The four pillars are given as **population, diversity, heredity, selection**,
  with common descent stated separately as background rather than as a pillar.
- **Selection operates on the phenotype**; **selection does not operate directly
  on the genotype**. Both stated explicitly.
- FPS probability: `Pr(i) = f_i / ?_j^? f_j`.
- Linear ranking: `P_LR(i) = (2 ? s)/? + 2i(s ? 1)/(?(? ? 1))`, with
  `s ? [1, 2]` identified as **selection pressure**.
- Survivor selection reduces **? parents and ? offspring to ? individuals**.
- A stated default mix: *"keep 10% elites, 40% mutated, 50% recombined."*
- Crossover probability `p_c ? [0.5, 1.0]`.
- *"Without recombination, evolution becomes a parallel gradient search."*

## Errors and things to watch

> [!warning] "Better at food gathering = better at surviving = make more offspring"
> Written as a chain of equalities. It is the **definition** of fitness being
> smuggled in as a causal claim, and as biology it is circular ? the individuals
> that leave more offspring are *defined* as fitter. The circularity is harmless
> in an algorithm, where the fitness function is stipulated by the engineer, and
> that is precisely the difference between the metaphor and the method. See
> [[fitness-function]].

> [!warning] Mutation is called "always stochastic: random and unbiased"
> **Unbiased** is not achievable in general and the lecture's own later advice
> contradicts it: for network mutation it recommends adding connections *"with
> weight zero or low weight"*, selecting nodes *"by taking into account the
> current connection scheme"*, and deleting with *"probability inversely
> proportional to weight"*. Every one of those is a deliberate bias. See
> [[mutation]].

> [!warning] `f(z) = 1/x?` in the fitness example
> Written with genotype `z` and phenotype `x ? ?`, so the function maps a
> genotype through an undeclared decoding step. The notes' `1/x?` also diverges
> at `x = 0` and is maximised there. The intent is clear enough ? smaller `x` is
> fitter ? but as written it is not a usable fitness function.

## Unclear in the source

- **No attribution anywhere.** Holland, Rechenberg, Fogel, Koza ? the four
  dialects have four originators and none is named. Consistent with the module's
  habit (see [[adam-kendon]]).
- The **roulette wheel** is described as having *"high variance from the
  theoretical distribution"* without stating the standard fix (stochastic
  universal sampling) [external], though *"expensive in distributed systems"* is
  given as the second objection.
- **Evolutionary Programming: "application-specific, e.g. FSM"** ? finite state
  machines are named here and nowhere else in the module.
- The **generative encoding / grammar** mentioned under *indirect mapping* is
  named in three words and never developed, though it is the most interesting
  idea on the page.
- The collision-free-navigation fitness function's terms are defined
  (`V` = rotation speed, `?V` = difference of speeds of wheels,
  `i` = activation of highest sensor) but **why it has that form** is not
  discussed.
- **No evaluation numbers anywhere**, again ? no convergence plots, no
  comparison against gradient methods, no problem sizes.

## See also

- [[overview]] ? [[evolutionary-algorithm]] ? [[neuroevolution]] ?
  [[L10-gesture-recognition]]
