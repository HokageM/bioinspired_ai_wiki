---
title: Genetic algorithm for inverse kinematics
type: system
sources: [L11]
tags: [evolution, robotics, optimisation]
updated: 2026-09-21
---

# A genetic algorithm for inverse kinematics

> **How to do genetic IK?**
>
> **In:** goal pose ? position in 3D space `(x, y, z)` + rotation in 3D space
> `(?, ?, ?)`
> **Out:** joint configuration for robotic arm `?j? ? j??`
> **Genetic encoding of representation:** for a pose `?j? ? j??`
> **Evaluation of individuals (fitness function):**
> - **Position and rotation error compared to the goal pose**
> - **Weight of position vs rotation error is a hyperparameter of the alg**

```
def fitness(genome, goal):          # genome = (j1 ... j6)
    pose = forward_kinematics(genome)          # cheap, exact, closed form
    e_pos = distance(pose.position, goal.position)
    e_rot = angular_distance(pose.rotation, goal.rotation)
    return -( w * e_pos + (1 - w) * e_rot )    # w: the hyperparameter
```

The encoding is as direct as it gets: the genome **is** the answer, six
real numbers, no decoding step. Contrast [[genetic-neural-encoding]], where the
layout of the genome is itself a design problem.

> [!note] The weight `w` is where the problem is actually specified
> Position error is in metres, rotation error in radians. They are not
> commensurable, so `w` is not a tuning knob but a **statement of what the task
> means** ? whether being 1 cm off matters more than being 1 degree off. Calling
> it *"a hyperparameter of the alg"* files a modelling decision as an
> implementation detail. The same criticism applies to every weighted-sum
> objective, and this is the module's clearest instance of one.

## The verdict

> **Pros:**
> - (+) **General heuristic for problem search**
> - (+) **Able to find good solutions in feasible computing time**
> - (+) **Distributed execution space**
>
> **Cons:**
> - (?) **No guarantee in optimal solution**
> - (?) **Long runtimes compared to search algorithms**
> - (?) **Success depends on used parameters**

> [!note] An unusually honest table, and the two middle entries contradict each other
> *"Feasible computing time"* sits directly above *"long runtimes compared to
> search algorithms"*. Both are true and the tension is the point: an EA is fast
> enough to be usable and slower than a method built for this problem. You pay
> that premium for **generality** ? the same code solves IK, navigation and
> network topology, and a dedicated IK solver solves IK.
>
> *"Success depends on used parameters"* is the honest admission underneath the
> whole lecture: population size, `p_c`, `p_m`, `k`, `w`, the elite fraction. The
> method trades the hyperparameters of a neural network for the hyperparameters
> of a search ? which is worth noting, since removing hyperparameters is how
> [[L11-evolutionary-computing]] motivated itself on page 1.

**"Distributed execution space"** is a real advantage and the only one that is
structural rather than empirical: fitness evaluations are independent, so the
expensive part parallelises perfectly. It is also the reason
[[tournament-selection]] is preferable to
[[roulette-wheel-selection]] here ? no global gather.

## See also

- [[inverse-kinematics]] ? [[evolutionary-algorithm]] ? [[fitness-function]] ?
  [[neuroevolution]]
