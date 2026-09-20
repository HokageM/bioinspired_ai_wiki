---
title: Pop-out effect
type: concept
sources: [L09]
tags: [attention, vision, perception]
updated: 2026-09-21
---

# Pop-out effect

> **Pop-out happens when an object has more salient physical features than other
> objects in the context.**
>
> **Physical features: location, colour, shape, orientation, brightness, etc.**

The notes sketch it: a field of identical oblique bars with one bar at a
different angle, which is found instantly regardless of how many bars there are.

## The crucial word is *context*

Saliency is **not a property of the object**. The same bar is invisible among
bars of its own orientation and unmissable among bars of another. What pops out
is a **local difference**, which is why every [[saliency-model|saliency model]]
computes **centre?surround differences** rather than absolute feature values.

That is also the connection the lecture does not make explicitly: centre?surround
is the [[the-retina|retina's]] organisation from L06, and orientation contrast
requires [[orientation-tuning|orientation-tuned]] V1 cells from L06. Pop-out is
what the L06 machinery does when you read out *difference from neighbours*
instead of *feature value*.

## The extension

> **Saliency could be extended to affective and social domain, like familiarity,
> threat, etc.**

> [!warning] This is a change of definition, not an extension
> Location, colour, orientation and brightness are computable from the image.
> Familiarity and threat are not ? they require **stored knowledge**, i.e. the
> *recognition network* that [[feature-integration-theory|FIT]] places
> **after** attention. If familiarity drives saliency, then saliency is not
> preattentive, and FIT's stage ordering is violated.
>
> The lecture offers the extension in one line, with no mechanism, no evidence
> and no reference, and does not notice the conflict with the theory on the
> facing page.

## In V1

> **Firing rates of V1's output neurons increase monotonically with the salience
> value of the visual input.**

The notes' illustration is exact: an input row of identical marks with one
different mark produces an output row where **only the odd one out is active**.
Uniformity suppresses, difference survives. See [[saliency-map]].

## See also

- [[saliency-map]] ? [[saliency-model]] ? [[winner-take-all]]
- [[feature-integration-theory]] ? [[attention]]
