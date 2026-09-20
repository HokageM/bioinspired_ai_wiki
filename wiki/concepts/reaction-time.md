---
title: Reaction time
type: concept
sources: [L09]
tags: [attention, methods]
updated: 2026-09-21
---

# Reaction time (RT)

> **Interval of time between the presentation of the stimulus and appearance of
> the appropriate voluntary response in the subject.**

The module's **first behavioural measure**. Everything before L09 measured
either neural activity (firing rates, fMRI) or model output (error, accuracy).
RT measures the *person*.

## Why it is powerful

RT is a scalar that is sensitive to the **number and difficulty of internal
processing stages**, none of which are observable. Manipulate one thing, measure
the change in RT, and you have evidence about machinery you cannot see. The
[[additive-factors-method]] is the inference rule that turns those changes into
claims about stages.

Note the definition's precision: *appropriate* and *voluntary*. Reflexes and
errors do not count, so RT is a measure of the whole
stimulus ? decision ? response chain, not of conduction speed.

## Where it is used

All three [[attention-networks|attention network]] scores are RT **differences**:

```
effect = RT(condition_without) - RT(condition_with)
```

Differencing cancels everything the two conditions share ? nerve conduction,
motor execution, individual baseline speed ? leaving only the contribution of
the manipulated factor. That is the whole design logic of the
[[attention-network-test]].

> [!warning] Direction is not consistent across the three
> A large alerting or orienting effect means the cue **helped**. A large
> executive control effect means conflict **cost** more ? worse performance. The
> three are presented as one family of difference scores without this being
> noted.

## Relation to the rest of the module

RT is the measurement that makes L07's **validation standard** ? models must
reproduce measured biological signatures ? achievable for attention. See
[[ann-brain-correspondence]]: the module's strongest correspondences are the
ones where *both sides produce a number*. RT is the number on the human side.

No model in L09 is actually tested against RT data. The tool is introduced and
not used.

## See also

- [[additive-factors-method]] ? [[attention-network-test]] ? [[attention-networks]]
