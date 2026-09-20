---
title: Curiosity and intrinsic motivation
type: concept
sources: [L13]
tags: [learning, agents, development, continual-learning]
updated: 2026-09-21
---

# Curiosity and intrinsic motivation

> - **Select strategies that maximize reward**
> - **Empirical process of exploration**
> - **Self-generation of a learning curriculum**

Diagram:

```
        Environment ???? external reward ?????
             ?                               ?
        Agent: strategy / action selection
             ?                               ?
        intrinsic motivation ??? internal reward
```

An agent driven by **two** reward signals: one from the world, one from itself.

## Self-generated curricula

The third bullet is the substantive one and the answer to the problem in
[[developmental-and-curriculum-learning]]. A curriculum needs someone to order the
tasks; requiring a designer to do it contradicts
[[continual-learning|continual learning]]'s premise. An intrinsic reward for
*novelty* or *learning progress* lets the agent order its **own** experience ?
seek what it is currently able to learn, leave what it already knows and what it
cannot yet handle [external].

That closes the loop the lecture opens and does not close.

> [!warning] This is a fully drawn reinforcement learner, and RL is still undefined
> Agent, environment, action selection, external reward, internal reward ? the
> diagram is an **actor?critic architecture with an intrinsic reward term**. It is
> the fourteenth and final appearance of reinforcement learning in this module,
> and there has still been no value function, no policy, no update rule, no
> Bellman equation, nothing.
>
> Thirteen lectures have drawn RL's architecture, named its components
> ([[class-activation-map|"critic maps"]], L12), used its vocabulary
> ([[evolutionary-algorithm|fitness as a scalar quality signal]], L11) and taught
> its trade-off ([[selection-pressure|exploration/exploitation]], L11) ? without
> ever writing down the algorithm. The module's largest single gap, and the last
> page spends its chance on a *variant* of the thing it never defined.
> See [[learning-paradigms]].

## See also

- [[developmental-and-curriculum-learning]] ? [[learning-paradigms]] ?
  [[selection-pressure]] ? [[embodiment-and-situatedness]]
