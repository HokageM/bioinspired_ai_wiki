---
title: Intelligent Behaviour
type: concept
tags: [foundations, agents]
sources: [L01, L05, L08]
created: 2026-09-20
updated: 2026-09-20
status: solid
---

# Intelligent Behaviour

The set of capabilities a bio-inspired artificial agent is expected to display,
used in the module as the target definition that everything else serves.

## Biological origin

The list is derived by looking at what animals demonstrably do, rather than by
starting from a formal definition of intelligence. L01's framing is explicitly
empirical: *consider findings about intelligent behaviour in nature* (L01).

## The capabilities (L01)

| Capability | Note |
|---|---|
| Make decisions | Selecting an action under uncertainty |
| Learn and develop | Change with experience; develop over a lifetime |
| Communicate and cooperate | Multi-agent; anticipates the swarm/collective material |
| React to something new | Novelty handling, not just interpolation |
| Interpret images and scenes | Perception as active interpretation |

## The two requirements

An approach counts as bio-inspired only if it (L01):

1. **Considers findings about intelligent behaviour in nature** — the biology is
   an input to the design, not a post-hoc analogy.
2. **Learns, represents and processes based on bio-inspired principles** — the
   *representation* and the *processing*, not only the objective.

Requirement 2 is the load-bearing one. It is what distinguishes, for example, a
[[spiking-neural-network]] (biological representation: spike times) from a
[[mcculloch-pitts-neuron]] network (biologically motivated, but representing
with real-valued activity levels). See [[neural-coding]] for that split.

## Computational form

Not an algorithm — this is a design criterion. As a checklist it can be applied
mechanically when evaluating whether a method belongs in this module:

```text
function is_bio_inspired(method):
    if not draws_on_finding_about_nature(method):
        return false                      # requirement 1
    if representation(method) is not bio_principled:
        return "bio-motivated only"       # requirement 2 partially met
    if processing(method) is not bio_principled:
        return "bio-motivated only"
    return true
```

## Where it appears in the module

- [[L01-introduction-to-bio-inspired-ai]] — stated as the module's target.

## See also

- [[place-cells]], [[grid-cells]] — L01's worked example of a natural solution.
- [[overview]] — how the module's methods map onto these capabilities.
- [[hybrid-spiking-localisation-network]] (L05) — the first system that clearly
  satisfies both requirements.

## The requirements tested, in L05

L05 is the first lecture that lets the two requirements bite.

| System | Representation follows biology? | Processing follows biology? |
|---|---|---|
| [[hybrid-spiking-localisation-network]] | **yes** — spike trains, tonotopic | **yes** — MSO/LSO/IC as in the brainstem |
| [[hybrid-acoustic-tracking]] | no — sampled buffers | no — `arccos` and an SRN |
| [[multi-layer-perceptron]] (L03) | partly — rate-coded units | no — global gradients |

By the strict reading, only the first qualifies. The others are *motivated* by
biology while computing in an entirely conventional way — which is exactly the
case requirement 2 was written to exclude.

The interesting defence comes from [[hybrid-architecture]]: the algorithmic
[[cross-correlation-localisation]] and the neural [[jeffress-model]] compute the
*same function*. If a system counts as bio-inspired when it performs the
computation biology performs — regardless of substrate — then the hybrid
qualifies after all.

That is a real fork in what "bio-inspired" means, and neither L01 nor L05
addresses it.

## Open questions / gaps

- The five capabilities are given without justification or source.
- No criterion is offered for *how much* biological fidelity requirement 2
  demands. The module later treats both rate-coded and spiking models as in
  scope, so the line is evidently soft.


## The hardest case: Braitenberg (L08)

[[L08-behaviour-based-robotics]] supplies the counter-example this page needs.

> **Behaviour is goal-directed, fast, flexible and adaptive ? might even appear
> intelligent. No cognitive processes. Agent is purely reactive.**

A [[braitenberg-vehicle]] has two sensors, two motors and four wires. It
supports every mentalistic description we normally treat as evidence of an inner
life ? it *fears*, *admires*, *explores* ? and contains nothing that could be a
representation of anything.

The lecture's accompanying claim is the general principle:

> **Complex behaviour may simply be the reflection of a complex environment.**

So observed complexity is evidence about the **agent?environment system**, not
about the agent. The inference *rich behaviour ? rich mechanism* is unsound, and
the vehicle is the proof.

> [!note] This cuts against the module's own habit
> [[ann-brain-correspondence]] tracks how often the module argues from
> resemblance. L08 is the strongest warning that behavioural resemblance is
> cheap ? and L07 is the strongest demonstration that *quantitative,
> counter-intuitive signature* resemblance is not. The two lectures are back to
> back and neither mentions the other.
