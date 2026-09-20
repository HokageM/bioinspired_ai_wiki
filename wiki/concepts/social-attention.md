---
title: Social attention
type: concept
sources: [L09]
tags: [attention, agents, robotics, multimodal]
updated: 2026-09-21
---

# Social attention

> **Social attention helps humans quickly learn how to interact with others,
> learn the language, and build social relationships.**
>
> **Most crucial manifestation: ability to follow others' eye gaze.**

## Why gaze is the crucial case

Gaze-following is **attention about attention**: reading where someone else is
attending, and going there. It converts a private internal state into a public
signal, which is what makes joint attention ? and therefore reference ? possible.

That is also why the definition mentions **learning the language**. To learn
that a word names a thing, you need to know *which* thing the speaker means, and
gaze is how that is disambiguated. This is the missing mechanism in
[[L04-embodied-language-processing]]'s grounding story: L04 had
[[imitation-network|imitation]] tie a symbol to a sensorimotor act but no way to
select the intended referent from a scene full of candidates. Gaze-following is
that selector, arriving five lectures later, unconnected.

## The robotics results

> **Can the robot's facial expression and gaze impact on human?human?robot
> collaboration?** ? **eye connection & trust**

Setup: **H1 actor** (arm move), **H2 guide** (arm move), **instructor
[[icub|iCub]]** (gaze shift, verbal instructions), in a loop of
*robot ? action/adaptation ? human ? perception/simulation ? robot*.

Findings, as recorded:

> - **Robot happy face ? faster completion**
> - **Initial gaze to guide ? rated more intelligent**
> - **Gaze familiarity increased performance and perception**

And the cooperation-game study with [[nao|NAO]]: see
[[human-robot-collaboration]].

## What the results actually are

Two dependent variables recur: **completion time** (objective) and **ratings of
the robot's intelligence** (subjective). They come apart ? initial gaze changed
the *rating* without any claim about the *time*.

> [!note] This is [[intelligent-behaviour]] measured rather than argued
> L01 asked what makes behaviour count as intelligent; L08's
> [[braitenberg-vehicle]] argued that appearing intelligent is cheap. L09 goes
> further and **measures the appearance**: a gaze shift, costing nothing and
> changing no capability, raises the rated intelligence of the same robot.
>
> Which is the strongest possible support for Braitenberg's point, arrived at
> empirically and never connected to it.

> [!warning] No numbers, no method, no statistics
> Three claimed effects, no effect sizes, no sample sizes, no tests, no
> citation. The wiki records them as *reported*, not as *established*.

## Relation to NICO

[[nico]]'s head has an **emotion display** and cameras for eyes ? the exact
apparatus these studies manipulate. L08 gives the hardware, L09 the experiment,
and neither mentions the other.

## See also

- [[human-robot-collaboration]] ? [[attention]] ? [[nico]] ? [[icub]] ? [[nao]]
