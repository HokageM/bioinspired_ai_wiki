---
title: Mutation
type: concept
sources: [L11]
tags: [evolution, optimisation]
updated: 2026-09-21
---

# Mutation

> **Variation operator: create new individuals from old ones.**
> **Aim: small (phenotypic) change in one individual.**
> **Always stochastic: random and unbiased changes.**
> **Depend on representation.**

Note the contrast with [[recombination]]: recombination takes **two** parents and
jumps; mutation takes **one** and steps. Together they are exploration at two
different scales.

Note also *"small **phenotypic** change"* ? the requirement is on the phenotype,
not the genotype, which is exactly the locality problem that condemns bit-strings
in [[candidate-representation]].

## Change of allele values

> **Bitwise mutation:** for every position, flip bit with probability `p_m`.
> **Random resetting / uniform mutation:** for every position, change value to a
> random value from the corresponding domain, with probability `p_m`.

```
def bitwise_mutation(genome, p_m):
    return [1-g if random() < p_m else g for g in genome]

def random_resetting(genome, domains, p_m):
    return [choice(domains[i]) if random() < p_m else g
            for i, g in enumerate(genome)]
```

`p_m` is per **position**, not per genome ? so the expected number of changes is
`p_m ? L`, and the same `p_m` means something different at different genome
lengths. The source does not remark on this.

> [!warning] Random resetting is not a small change
> *"Change value to a random value from the corresponding domain"* replaces a
> gene with an arbitrary one, which for a real-valued domain is a jump of
> arbitrary size. That directly contradicts the stated aim, *"small (phenotypic)
> change"*. A Gaussian perturbation is the operator that actually meets the aim
> [external], and it is not mentioned.

## Mutating neural networks

> **Using only standard mutation (small changes to values in the representation)
> without recombination, evolution becomes a parallel gradient search.**
> Usually several mutation operators applied for different types of mutation.
> Operators: **change parameter values, add/delete nodes/connections.**

> [!note] The sharpest sentence in the lecture
> Drop recombination and keep small mutations, and an EA is `?` hill-climbers run
> in parallel with periodic culling ? no crossover means no information ever
> moves **between** lineages. The population stops being a population and becomes
> a batch.
>
> It also locates the value of recombination precisely: it is the *only* thing
> making an EA different in kind from parallel local search. And it explains
> [[fitness-landscape|why the jump must be destructive]] ? a move that is never
> destructive is never non-local either.

### Mutating connections

> **Naive random method:**
> - *Add:* select 2 random nodes ? add connection with random weight
> - *Delete:* remove random connection
>
> **Better:**
> - *Add:* add connection with **weight zero or low weight**; select nodes by
>   **taking into account the current connection scheme**
> - *Delete:* probability **inversely proportional to weight** of connection;
>   remove random connection with weight below threshold

> [!success] This is the answer to "how do you mutate a structure?"
> The naive method violates the aim ? bolting on a random-weight connection
> changes the network's function **immediately and arbitrarily**. Adding it with
> **weight zero** changes the function *not at all*: the topology grows, the
> behaviour is preserved, and evolution can tune the new weight afterwards.
>
> Deleting in inverse proportion to weight is the same principle in reverse:
> remove what is barely contributing, so the phenotype hardly moves.
>
> Both are **structural changes that are phenotypically neutral at the moment
> they occur** ? which is the only way to satisfy *"small phenotypic change"*
> while altering topology.

> [!warning] And both contradict "random and unbiased"
> The page opens by asserting mutation is *"always stochastic: random and
> unbiased"* and then recommends four deliberate biases. The recommendations are
> right; the characterisation is wrong.

### Mutating the number of nodes

> - **Naively add/delete random nodes leads to varying results**
> - **Consider:** outgoing connections, number of incoming, node parameters
>   *(marginal: "mind incoming growth")*
> - **General: calculate probability depending on (weights, topology);
>   `Prob(delete) > Prob(add)`**

The final rule is a **parsimony pressure**: bias the process towards shrinking,
or networks grow without bound because extra capacity rarely hurts fitness
directly. It is the same concern that [[gwr-network|GWR]] handles with its firing
counter and edge ageing ? two lectures, two mechanisms, one problem, no
cross-reference.

## See also

- [[recombination]] ? [[genetic-neural-encoding]] ? [[neuroevolution]] ?
  [[candidate-representation]]
