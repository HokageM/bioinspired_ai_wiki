---
title: Feature integration theory (FIT)
type: concept
sources: [L09]
tags: [attention, vision, representation]
updated: 2026-09-21
---

# Feature integration theory (FIT)

The stage model of visual attention. Unattributed in the source ? no name, no
year.

```
Object
  ?
Preattentive stage        ? analyse into features
  ?
Focused attention stage   ? feature binding
  ?
Perception  (Wahrnehmung)
```

## The architecture

Separate **feature maps** ? colour maps, orientation maps ? all registered
against a shared **map of locations**. Attention is applied *at a location*; the
features present at that location are read out together and become a

> **temporary object representation (Time t, Place x)**

which is matched against a **recognition network** ? *stored descriptions of
objects, with names*.

## The claim that makes it a theory

**Features are free; conjunctions are not.** Colour is computed everywhere at
once, orientation is computed everywhere at once, but *which colour goes with
which orientation* requires attention at a location. Binding is the bottleneck.

This explains [[pop-out-effect|pop-out]] and its absence in one stroke: a target
differing in a **single** feature is found preattentively, in parallel; a target
defined by a **conjunction** of features requires the spotlight to visit
candidates one at a time.

> [!note] The location map is doing the binding
> Nothing binds colour to orientation directly. They are bound by **sharing a
> place** ? the location map is the common index. This is a
> [[place-cells|topographic code]] used as a **join key**, which is a job the
> module's other five topographic maps are never given.

## The two representations

| | Temporary object representation | Recognition network |
|---|---|---|
| Content | features at (Time t, Place x) | **stored descriptions of objects, with names** |
| Lifetime | momentary | persistent |
| Built by | attention | learning |

*With names* is the interesting phrase: this is where perception meets
[[embodied-language-representation|language]], and it is the same junction
[[nico]] needs for *"fusing vision, motor and semantics"*. Neither lecture cites
the other.

## Pseudocode

```
def fit(image, target=None):
    maps = {f: feature_map(image, f) for f in (COLOUR, ORIENTATION, ...)}
    locations = location_map(maps)                  # shared spatial index

    if is_single_feature(target):                   # pop-out: parallel
        return argmax(maps[target.feature])

    for loc in attention_scan(locations):           # conjunction: serial
        bundle = {f: maps[f][loc] for f in maps}    # binding happens HERE
        obj = TemporaryObjectRepresentation(t=now(), place=loc, features=bundle)
        if match(recognition_network, obj):
            return obj
```

## What is not given

- No attribution, date or citation.
- No account of **illusory conjunctions** ? the prediction that unattended
  features mis-bind, which is the theory's main empirical support.
- The relationship between FIT and the [[saliency-model]] on the same page is
  not stated, though saliency is the obvious mechanism for deciding where the
  spotlight goes next.

## See also

- [[pop-out-effect]] ? [[saliency-map]] ? [[attention]]
- [[orientation-tuning]] ? the feature maps FIT assumes, from L06
