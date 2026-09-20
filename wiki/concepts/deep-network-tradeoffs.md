---
title: Deep network trade-offs
type: concept
sources: [L10, L11]
tags: [learning, training, plausibility, methods]
updated: 2026-09-21
---

# Deep network trade-offs

[[L10-gesture-recognition]]'s **evaluation of alternatives for deep neural
networks** ? the module's only systematic cost/benefit account of the method it
has used since L03.

## In favour

| | |
|---|---|
| **Successful benchmarking** | in vision and audio domains |
| **Robust feature emergence** | due to hierarchical processing (*Entstehung*) |
| **Optimisation efforts in open software** | the ecosystem, not the science |
| Trends | transfer learning, generative and unsupervised models |

## Against

| | |
|---|---|
| **Data-hungry and time-consuming** | exhaustive tuning, retraining, optimisation strategies |
| **Specialist for one task** | *(adaptivity? feedback?)* |
| **Not fully appropriate for the temporal domain** | **3D kernel: no significant advantage over 2D kernels** |

## The three objections, read carefully

**Data-hungry.** An engineering cost, but also a
[[ann-brain-correspondence|plausibility]] problem: a child learns a gesture from
a handful of examples. The module has flagged sample efficiency nowhere else.

**Specialist for one task.** The parenthetical *"adaptivity? feedback?"* is the
real objection: a trained network is **static**. It does not continue to learn,
does not adapt to a new user, and has no feedback loop with the world. That is
the same complaint as [[task-inference-network]]'s *"fixed task representations
are incompatible with continual learning"* (L08) ? and it motivates
[[gwr-network|GWR]], which is the direct answer.

**Not appropriate for the temporal domain.** The only **negative result** stated
anywhere in the module: a 3D kernel is not worth its cost. It has a *fixed*
temporal extent, so it is time-**aware** but not time-**invariant**; a
recurrent state is. Hence [[cnn-lstm]].

> [!note] The critique generates the rest of the lecture
> Each objection is answered by the next architecture:
>
> | Objection | Answer |
> |---|---|
> | not appropriate for time | [[cnn-lstm]] ? put memory in the model |
> | data-hungry | [[openpose]] ? move work into preprocessing, shrink the input |
> | specialist, not adaptive | [[gwr-network]] ? a network that grows |
>
> This is the clearest example in the module of an architecture being
> **derived from a stated failure** rather than asserted.

## What is not said

- **No numbers.** "Data-hungry", "no significant advantage" ? no benchmark, no
  ablation, no citation.
- **The alternatives have costs too.** GWR introduces thresholds and a growth
  policy; OpenPose is itself a large deep network, so the data cost is paid
  once, elsewhere, by someone else ? not removed.

## See also

- [[multichannel-cnn]] ? [[cnn-lstm]] ? [[gwr-network]] ? [[learning-paradigms]]


## L11 ? the hyperparameter objection, answered by search

L10's third objection is *"specialist for one task (adaptivity? feedback?)"*, and
underneath it sits a question the module had never asked:
**who chooses the architecture?**

[[L11-evolutionary-computing]] asks it directly ?

> **How to select necessary topology, weights and efficient learning
> parameters?**

? and answers: search for them. See [[neuroevolution]].

> [!note] Two answers to the architecture question in two consecutive lectures
> | | [[gwr-network|GWR]] (L10) | [[neuroevolution]] (L11) |
> |---|---|---|
> | When | during learning, online | across generations |
> | Driven by | coverage of the input | fitness of the whole network |
> | Candidates | one network | a population |
> | Needs | a distance metric | a scalar score |
> | Cost | cheap, incremental | many full evaluations |
>
> Neither lecture mentions the other, and the two are complementary rather than
> competing: GWR adapts capacity to the *data*, neuroevolution adapts it to the
> *task*.

> [!warning] And the cost is honestly stated
> [[genetic-inverse-kinematics]]: *"success depends on used parameters."* An EA
> removes the neural network's hyperparameters by introducing its own ?
> population size, `p_c`, `p_m`, `k`, elite fraction. The data-hunger of L10
> becomes the evaluation-hunger of L11. Neither lecture notes that the problem
> has been moved rather than eliminated.
