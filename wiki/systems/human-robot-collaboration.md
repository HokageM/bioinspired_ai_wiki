---
title: Human?robot collaboration studies
type: system
sources: [L09]
tags: [robotics, agents, attention, methods]
updated: 2026-09-21
---

# Human?robot collaboration studies

> **Social attention in a human?robot cooperation game.**

A three-step research design, which is the interesting part:

| Step | What is done |
|---|---|
| **Human?human interaction** | human participants played the game with human assistants of **different personalities** (introverted / extroverted / neutral) |
| **Modelling social cues** | **model the social cues of the human assistants into robots**, and design a **decision-making algorithm for the most suitable assistive action** |
| **Human?robot interaction** | human participants played with [[nao|NAO]] robot assistants with different personalities (intro / extro) and **different autonomy** (with `WoZ` ? a Wizard-of-Oz condition) |

## Why the three steps matter

This is the module's only **complete methodological loop**: observe humans ?
extract a model ? implement it ? test it against humans again. Every other
"bio-inspired" claim in the module runs the first and third steps informally, or
skips straight to implementation.

> [!note] It is the L07 validation standard applied to behaviour
> [[ann-brain-correspondence]] records L07's standard ? a model counts as
> brain-like if it **reproduces measured signatures**. This design is the social
> version: measure human assistants, model them, and check the robot produces
> comparable interaction outcomes. The module now has the same argumentative
> form at two very different levels, neuronal and interpersonal.

## The decision-making algorithm

> **Verbal and non-verbal cues for both extroverted and introverted robots.**
> Decision-making algorithm:
> - **Personality-based**
> - **State-based**

```
# The two named policies, as far as the source specifies them
def assist_personality_based(context, personality):
    return policy[personality](context)      # fixed disposition, ignores state

def assist_state_based(context, state):
    return policy(state(context))            # responds to the situation
```

The distinction is real and familiar: a **trait** policy is constant and
predictable; a **state** policy is reactive and context-sensitive. It is the same
fork as [[exogenous-and-endogenous-attention|goal-driven versus stimulus-driven]]
and as [[reactive-agent|reactive versus deliberative]] control, now at the level
of social behaviour.

> [!warning] Neither algorithm is specified
> Two bullet points. No inputs, no action set, no selection rule, no results.
> `WoZ` is never expanded. [external] It conventionally denotes *Wizard of Oz* ?
> a human secretly operating the robot ? which would make it the autonomy
> manipulation; the notes do not say so.

## Conclusion of the lecture

> - Human attention theories
> - Psychological and neural mechanisms underlying visual, auditory, and
>   audiovisual crossmodal selective attention
> - Bio-inspired models, cognitive simulation, and human?robot interaction
> - **Comparison between human behaviour and modelling / robot performance**

The last line is the lecture's own statement of the validation standard.

## See also

- [[social-attention]] ? [[nao]] ? [[icub]] ? [[nico]]
- [[ann-brain-correspondence]]
