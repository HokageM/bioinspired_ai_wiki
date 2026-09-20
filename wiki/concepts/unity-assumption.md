---
type: concept
tags: [multimodal, perception]
sources: [L07]
status: solid
---

# Unity assumption

> **Whenever two or more sensory inputs are perceived as being highly
> consistent, observers will be more likely to treat them as referring to a
> single multisensory percept with a common spatiotemporal origin.**
> ? [[L07-crossmodal-processing]], p4

```
consistent    ?  unification  ?  one multisensory percept
inconsistent  ?  segregation  ?  separate visual percept + auditory percept
                 ("prior entry")
```

## The problem it solves

Every other idea in the lecture assumes the signals being combined **belong
together**. [[optimal-cue-integration]] weights two estimates of *the same
quantity*; the [[spatial-principle]] enhances stimuli from *the same event*. But
a perceptual system is not handed that guarantee. At any moment several things
are making noises and several things are visible, and combining the wrong pair is
worse than not combining at all.

The unity assumption is the gate in front of integration: **decide whether these
signals share a cause, and only then fuse them.**

This makes it logically prior to everything else in the lecture, and the lecture
introduces it fourth.

## It is the same thing as depression

The [[spatial-principle]] says displaced stimuli produce **depression** ? a
response *below* either unimodal baseline. That is segregation implemented in
firing rates. A column whose evidence is contradicted by a competing location is
suppressed rather than averaged.

So the [[superior-colliculus]] does not apply the unity assumption as a separate
test; its columnar geometry enforces it automatically. Consistent inputs share a
column and reinforce; inconsistent inputs occupy different columns and compete.
The notes give both facts and never connect them.

## Pseudocode

```
function perceive(x_v, x_a):
    if consistent(x_v, x_a):
        return unify(integrate(x_v, x_a))     # one percept
    else:
        return [percept(x_v), percept(x_a)]   # two percepts, segregated

function consistent(x_v, x_a):
    return  spatial_distance(x_v, x_a)  < spatial_window
        and temporal_distance(x_v, x_a) < temporal_window
```

```
# The softer, and more defensible, version: causal inference.  [external]
# Rather than a hard test, compute the probability of a common cause and
# let it weight the blend continuously:
#
#   p_common = P(one source | x_v, x_a)
#   percept  = p_common * integrate(x_v, x_a)
#            + (1 - p_common) * segregate(x_v, x_a)
#
# This explains partial capture, which a hard gate cannot. The notes'
# phrase "more likely to treat them as" hints at it without formalising.
```

## Unclear in the source

- **"Consistent" is not operationalised** ? no window in space or time, no
  metric, no threshold.
- **"Prior entry"** appears once, attached to segregation, with no definition.
  `[external]` Prior entry is normally the finding that an *attended* stimulus
  is perceived as occurring earlier than an unattended one ? a temporal effect,
  not obviously a segregation mechanism. Its role here is unclear.
- **The phrase "more likely"** implies a graded, probabilistic process, but the
  diagram is a hard binary fork. The tension is not addressed.
- **Never linked to the [[ventriloquism-effect]]**, though it is what licenses
  the puppet and the voice to bind in the first place.

## Related

[[multisensory-integration]] ? [[spatial-principle]] ?
[[optimal-cue-integration]] ? [[ventriloquism-effect]] ?
[[superior-colliculus]] ? [[L07-crossmodal-processing]]
