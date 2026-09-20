---
title: Additive-factors method
type: concept
sources: [L09]
tags: [attention, methods, plausibility]
updated: 2026-09-21
---

# Additive-factors method

> **Procedure for analysing reaction-time data to determine whether two
> variables affect the same or different processing stages.**
>
> - **If two variables influence different stages, their effect should be
>   additive**
> - **If two variables influence the same stage, their effect should be
>   interactive**

## The idea

If processing runs as a chain of stages and each stage takes some time, total
[[reaction-time|RT]] is the **sum** of stage durations. Slow down stage 1 by
`a`, slow down stage 3 by `b`, and total RT rises by `a + b` ? the effects do
not interact, because the stages do not.

If both manipulations hit the **same** stage, they act on the same computation
and their joint effect is whatever that computation does with both ? generally
not the sum.

```
# Different stages: additive
RT(a, b) = base + a + b          =>  RT(a,b) - RT(a,0) - RT(0,b) + RT(0,0) == 0

# Same stage: interactive
RT(a, b) != base + a + b         =>  the interaction term is non-zero
```

Operationally: run a 2?2 factorial, fit an ANOVA, and **look at the interaction
term**.

## The logical problem

> [!warning] The method is stated as a biconditional and used in reverse
> The sound direction is: *separate stages ? additive effects.* The lecture then
> reads it backwards ? observe additivity, conclude separate stages ? which is
> affirming the consequent.
>
> Two factors can act on the **same** stage and still be additive: if that
> stage's duration happens to be linear in both, the interaction term is zero.
> Additivity is **consistent with** separate stages; it does not establish them.
>
> The method also presupposes what it is used to argue for: that processing is
> **serial and discrete**. If stages overlap in time, or if one can start before
> the previous finishes, RT is not a sum of durations and the whole inference
> fails.

This matters for the [[attention-networks|three-network model]], whose
independence is exactly the claim at stake and is exactly what the
[[attention-network-test]] assumes when it extracts three scores from one task.

## Why it is worth keeping anyway

It is the module's only stated **method for inferring internal structure from
external measurements** ? the same problem as
[[ann-brain-correspondence|establishing that a model matches a brain]], and the
same problem [[lime]] (L06) attacks for networks. The three approaches:

| Approach | Lecture | Infers structure from |
|---|---|---|
| Additive factors | L09 | RT under factorial manipulation |
| [[lime]] | L06 | perturbing inputs, refitting locally |
| Signature reproduction | L07 | matching measured phenomena |

All three are perturb-and-observe. None can see inside.

## See also

- [[reaction-time]] ? [[attention-network-test]] ? [[attention-networks]]
