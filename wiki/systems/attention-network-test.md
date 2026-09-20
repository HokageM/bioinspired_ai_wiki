---
title: Attentional Network Test
type: system
sources: [L09]
tags: [attention, methods]
updated: 2026-09-21
---

# Attentional Network Test (ANT)

One task that yields three scores, one per
[[attention-networks|attention network]], using
[[reaction-time|RT]] differences and the [[additive-factors-method]].

```
Alerting effect          = RT(no cue)       ? RT(double cue)
Orienting effect         = RT(centre cue)   ? RT(spatial cue)
Executive control effect = RT(incongruent)  ? RT(congruent)
```

## Reading the three contrasts

**Alerting** ? a *double cue* warns that something is about to happen but not
where. Subtracting it from the uncued condition isolates the benefit of **being
ready**, with no spatial information involved.

**Orienting** ? a *centre cue* says *now* but not *where*; a *spatial cue* says
*there*. The difference isolates the benefit of **knowing the location**, with
alertness held constant. The design is careful: both conditions are cued, so
alerting cancels.

**Executive control** ? congruent versus incongruent stimuli isolates the cost
of **resolving conflict**.

```
for trial in trials:
    cue = choose(NO_CUE, DOUBLE_CUE, CENTRE_CUE, SPATIAL_CUE)
    target = choose(CONGRUENT, INCONGRUENT)
    rt[cue][target] = present_and_time(cue, target)

alerting  = mean(rt[NO_CUE])     - mean(rt[DOUBLE_CUE])
orienting = mean(rt[CENTRE_CUE]) - mean(rt[SPATIAL_CUE])
executive = mean(rt[INCONGRUENT]) - mean(rt[CONGRUENT])
```

> [!warning] The third score points the other way
> Larger alerting and orienting effects mean the cue **helped** ? better
> function. A larger executive control effect means conflict **cost more** ?
> worse function. Three numbers, presented as a family, with opposite polarity
> in the third.

> [!warning] The design assumes what it measures
> Extracting three independent scores from one task is only valid if the three
> networks are **separable stages**. That is precisely the claim the
> [[additive-factors-method]] is supposed to test, and here it is presupposed.
> The notes give no additivity check.

## What is missing

The notes give the three formulas and nothing else: no stimulus (the conflict
manipulation is unspecified), no timings, no number of trials, no results, and
no reference. The ANT is named in the *take-home message* as the thing the RT
method is for.

## See also

- [[attention-networks]] ? [[reaction-time]] ? [[additive-factors-method]]
