---
title: Embodiment and situatedness
type: concept
sources: [L08, L11]
tags: [robotics, embodiment, agents, foundations]
updated: 2026-09-21
---

# Embodiment and situatedness

The two **core ideas of a behaviour-based system**, per
[[L08-behaviour-based-robotics]].

## Embodiment

> **Robot has a real body: the body and the internal state of this body
> influence higher cognitive functions and the resulting behaviour.**

Marginal gloss: *physical interaction through body.*

The claim is not that a body is convenient. It is that the body is **part of the
computation** ? morphology, mass, joint limits and sensor placement determine
what the controller has to do, and a great deal of apparent control is actually
done by the body for free.

## Situatedness (embeddedness)

> **Robot is in a real situation: robot must deal with sensory and motor
> eventualities where it operates and not with abstract descriptions of the
> world, i.e. physical interaction between the body and the world strongly
> constrain the possible behaviour.**

Marginal gloss: *robot operates directly with and in the world.*

The operative phrase is **not with abstract descriptions**. This is
[[functional-decomposition]]'s world model, rejected by name.

## Why the two together are an argument, not a pair of slogans

Embodiment says the body does part of the work. Situatedness says the world does
part of the work. What is left for the controller is much less than the
classical pipeline assumes ? which is why a [[braitenberg-vehicle]] with four
wires can produce behaviour one would otherwise expect to require a planner.

> **Complex behaviour may simply be the reflection of a complex environment.**

## Relation to L04's embodiment

[[L04-embodied-language-processing]] used *embodiment* for a claim about
**meaning**: concepts are grounded in sensorimotor experience, and
[[embodied-language-representation]] is the module's version of it.

L08 uses the same word for a claim about **control**: the body constrains and
simplifies the action problem.

| | L04 | L08 |
|---|---|---|
| Claim about | representation / semantics | control / behaviour |
| Body supplies | grounding for symbols | constraint on what must be computed |
| Opponent | amodal symbol systems | the world model |

They are compatible and mutually supporting ? grounding requires
[[imitation-network|interaction]], which requires a body that acts ? but the
module never puts them side by side. Recorded here because a reader meeting
"embodiment" twice, four lectures apart, in two different senses, deserves to be
told.

> [!note] Neither lecture cites the other
> L04 develops embodiment for language without mentioning robots; L08 develops
> it for robots without mentioning language. [[nico]] is where they would meet ?
> a body, with hands, learning to pick objects named in words ? and the lecture
> does not make the connection.

## See also

- [[reactive-agent]] ? [[subsumption-architecture]] ? [[nico]]
- [[embodied-language-representation]] ? the L04 sense


## L11 ? embodiment as a fitness function

[[collision-free-navigation]]'s objective is written entirely in terms of the
robot's **own body and sensors**:

`? = V (1 ? ??V)(1 ? i)` ? wheel speed, difference between the two wheels,
activation of the most-stimulated proximity sensor.

No world model, no map, no goal location, no representation of an obstacle. The
task is specified in the quantities the agent can actually measure about itself,
and *navigation* is what emerges from maximising it.

> [!success] L08's thesis, used as an engineering method
> ~~Behaviour-based robotics argues that intelligent behaviour arises from a
> simple agent coupled to a complex environment, without internal
> representation ? but the argument is made through demonstrations
> ([[braitenberg-vehicle]], [[subsumption-architecture]]) rather than as a
> constructive procedure.~~
>
> L11 supplies the procedure. **Because** behaviour is a property of the
> agent-environment coupling rather than of an internal model, you can specify a
> local, embodied quality measure and let [[evolutionary-algorithm|search]] find
> the coupling. Situatedness is what makes the fitness function short enough to
> write down.

> [!note] It also makes the environment part of the algorithm
> Fitness is measured by *running the robot*, so the environment is inside the
> optimisation loop. An EA on an embodied task is not optimising a function of
> the genome alone ? it is optimising a function of genome **and** world, which
> is situatedness stated formally. The lecture does not put it this way.
