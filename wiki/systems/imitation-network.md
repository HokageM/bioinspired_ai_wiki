---
type: system
title: Imitation Network (supervised neural controller for grounding)
sources: [L04, L08]
tags: [robotics, embodiment, grounding, supervised, imitation-learning]
---

# Imitation Network

L04's answer to *from basic to higher-level grounding*: a **supervised neural
controller for grounding**, demonstrated with stick-figure robots.

## The setup

- An **epigenetic autonomous robot** learns basic actions **and their names**.
  - It reflects **semantic associations at the sensorimotor level**.
  - *Linguistic abilities strictly depend on motor skills and behaviours.*
- Modelled with **stick-figure robots based on an NN controller**.
- Two roles: **demonstrator** and **imitator**.

```
Demonstrator's joint angles ┐
                            ├→ Imitation Algorithm →→ Motors →→ Motor Output
Imitator's joint angles     ┘                          ↑
                                          Verbal Instruction Parser
```

- **Dense feed-forward NN** — i.e. an [[multi-layer-perceptron]] trained by
  [[backpropagation]].
- **Four pairs of motor neurons.**
- **Imitation algorithm:**
  - estimate the necessary force of each motor joint
  - **minimise the difference** for motor output

## Pseudocode

```
# ---- imitation as supervised control ----
# theta_D : demonstrator joint angles (the target posture)
# theta_I : imitator joint angles     (the current posture)
# word    : action word spoken alongside the demonstration

# --- Stage 1: basic grounding ---
for each demonstration (theta_D_sequence, word):
    reset imitator to the SAME start position as demonstrator

    for each timestep t:
        obs    <- concat( theta_D[t], theta_I[t], parse(word) )
        forces <- controller(obs)              # dense feed-forward NN
        theta_I[t+1] <- physics(theta_I[t], forces)

        # supervised: the demonstrator's own trajectory IS the target
        loss <- norm( theta_I[t+1] - theta_D[t+1] )
        update controller by gradient descent on loss

    # the word is bound to the sensorimotor trace it co-occurred with
    associate(word, trajectory_representation(theta_I))

# --- Stage 2: higher-order grounding by transfer ---
# freeze the primitives, compose them
for each behaviour (sequence_of_known_actions, natural_language_description):
    train only the composition layer on top of the frozen action primitives
    associate(description, the composed sequence)
```

The **same start position** requirement is doing real work: it is what makes the
demonstrator's joint angles usable directly as targets, with no coordinate
transform. This is imitation in the cheapest possible sense — the hard problem
of *correspondence* (mapping another body's frame onto your own) is sidestepped.

## Learning: basic grounding

The imitator learns to execute actions by:

1. **same start position**
2. **observing the demonstrator**
3. **mimicking movement**
4. **learning action and names simultaneously**

⇒ **Grounding of words in perception and the imitator's production.**

Action words given as examples: *open*, *left*, *turn*.

## Transfer: higher-order grounding

- The imitator learns **behaviours**: combined actions, with a **linguistic
  description in natural language**.
- ⇒ **Grounding of names and concepts of new actions.**

This is [[transfer-learning]]: primitives learned in stage 1 are reused to
ground vocabulary that was never demonstrated directly.

## Results

- Language representations allow **composing complex linguistic constructions
  from primitives** ⇒ **compositional language structure**
  ([[compositionality-of-language]]).
- Grounding: *result not just accurate but also **contextually correct***.

## The methodological tension

This is the module's most explicit model of [[symbol-grounding]] — and it is
**supervised**. The demonstrator supplies the target trajectory; the network
minimises a difference. Compare:

| Model in L04 | Learning | Grounded in |
|---|---|---|
| [[multi-layer-associator]] | Hebbian, local | co-activation |
| [[self-organising-map]] | unsupervised, competitive | input topology |
| **Imitation network** | **supervised** | **sensorimotor experience** |
| [[word2vec]] / [[gpt]] | self-supervised, gradient | text statistics only |

So the most *embodied* model is also the least *biologically plausible in its
learning rule*. Infants are not given their parents' joint angles as a target
vector. See [[ann-brain-correspondence]].

## Unclear in the source

- **"Four pairs of motor neurons"** is the only architectural detail; no layer
  sizes, no activation functions.
- The **Verbal Instruction Parser** is drawn feeding the motors but never
  described.
- "Epigenetic" is used without definition. In developmental robotics it means
  *acquiring capabilities through interaction with the environment over
  time* `[external]`; the notes do not say.
- How the word→trajectory association is actually stored is not specified.

## See also

[[symbol-grounding]] · [[transfer-learning]] ·
[[compositionality-of-language]] · [[embodied-language-representation]] ·
[[multi-layer-perceptron]] · [[backpropagation]] ·
[[L04-embodied-language-processing]]


## Imitation for a different purpose in L08

[[L08-behaviour-based-robotics]] uses *imitation* to mean something else:
**copying a task**, not copying a signal.

| | L04 (this page) | L08 [[imitation-learning]] |
|---|---|---|
| What is copied | the perceived signal, to ground it | the structure of a demonstrated task |
| Output | a representation | a symbolic action plan |
| Needs symbols | no | **yes** |
| Timescale | one item | a temporally extended sequence |

Both are called imitation learning; only one is compatible with a
[[reactive-agent]]. L08's version requires estimating *objects and transitions*
and reasoning symbolically over them ? the very world model the same lecture's
first half argues against.
