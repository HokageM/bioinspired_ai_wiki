---
title: Unsupervised task inference
type: system
sources: [L08, L10]
tags: [robotics, learning, unsupervised, agents, self-organisation]
updated: 2026-09-21
---

# Unsupervised task inference in continual robot learning

## Motivation

> - Learning **predefined** tasks ? learning **a growing set of tasks**
> - To learn new tasks, the robot first needs to **infer the task at hand**
> - **Fixed task representations (e.g. labels) are incompatible with continual
>   learning**
> - **Existing approaches require extensive exploration to infer a task before
>   executing the learned policy**
> - **Observing a demonstration**

The middle two are the problem statement, and they are sharp. A fixed label set
cannot grow; and if inferring *which* task you are in requires exploring, the
robot must behave badly before it can behave well.

## Overview

```
{ Task demonstrations }  ?  Self-Organised Network of Behaviours
                          ?  Task inference
                          ?  Task-conditioned control (policy network)
```

The notes sketch the intermediate stage as a graph of linked nodes and the task
as a small subgraph ? a path `a ? b ? c` through it.

## Reading it

```
# Training
network = self_organise(demonstrations)      # behaviours arrange themselves
                                             # by similarity; no task labels

# Deployment
def act(observation):
    task = infer_task(network, observation)  # a point/region, not a label
    return policy(observation, task)         # task-conditioned control
```

The design move is to replace a **discrete label** with a **position in a
learned space**. A label set is fixed the moment it is written down; a space can
accommodate a new task as a new region, and ? the real payoff ? a *nearby* task
inherits a nearby conditioning vector, so the policy generalises to tasks it was
never trained on.

> [!note] "Self-Organised Network of Behaviours" is almost certainly a SOM
> The phrase, the graph-of-linked-nodes diagram, and the requirement for
> unsupervised topology-preserving clustering all point at the
> [[self-organising-map]]. **The lecture never says so**, gives no algorithm, no
> update rule and no learning rate, so the wiki does not assert it.
>
> If it is, this would be the SOM's **fourth** distinct job in the module:
> feature organisation (L04), cross-modal mapping (L04), probabilistic
> [[histogram-based-som|sensory fusion]] (L07) ? and now organising a space of
> **behaviours** rather than of percepts. That the same mechanism keeps being
> reached for is the closest thing the module has to a unifying algorithm, and
> it is never remarked upon.

## What is missing

- The learning rule. Everything above is read off a three-box diagram.
- How `infer_task` works at run time from a single observation, when the
  motivation section's complaint was precisely that inference usually needs
  exploration.
- How the policy network is conditioned, or trained.
- Any evaluation, task set, or result.

## See also

- [[self-organising-map]] ? [[imitation-learning]] ? [[nico]]
- [[learning-paradigms]] ? *continual learning* is named here for the first time
  and is not in L03's taxonomy of supervised / unsupervised / reinforcement


## L10 ? the likely identity of the self-organised network

L08 describes *"a self-organised network of behaviours"* that supports continual
learning, and names no algorithm. [[L10-gesture-recognition]] introduces
[[gwr-network|GWR]], whose stated motivation is exactly the objection L08 raises:

> **[L08] Fixed task representations are incompatible with continual learning.**
> **[L10] SOM size is driven by the input and not fixed. Growth and shrinking.**

A network that adds nodes when the input is poorly covered can acquire a new
behaviour without being resized or retrained ? which is what L08 wanted and could
not get from a fixed [[self-organising-map|SOM]].

> [!note] An inference, not a statement
> The two lectures are two weeks apart and do not reference one another. The wiki
> records this as a likely identification because the problem, the solution and
> the research group all line up ? not because either source says so.
