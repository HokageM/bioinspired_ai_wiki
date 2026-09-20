---
type: concept
tags: [multimodal, statistics, perception]
sources: [L07]
status: solid
---

# Optimal cue integration

The second explanation offered for the [[ventriloquism-effect]], introduced in
[[L07-crossmodal-processing]] p2 as the **alternative hypothesis: integration**,
and returning on p5 as the **ML estimator model** that the
[[histogram-based-som]] is checked against.

> **Small errors in visual localisation and large errors in auditory
> localisation lead to optimal localisation being very close to the visual
> estimate, hence the ventriloquism effect.**

## The idea

Each modality gives a noisy estimate of the same quantity. If you know how noisy
each one is, there is a single best way to combine them: **weight each estimate
by its reliability, where reliability is inverse variance.**

The notes state this verbally and never write it down. The formalisation below
is `[external]`, but it is the standard one and it reproduces the notes' claim
exactly.

```
Given   x_v ~ N(s, ?_v?)     visual estimate
        x_a ~ N(s, ?_a?)     auditory estimate

weights w_v = (1/?_v?) / (1/?_v? + 1/?_a?)
        w_a = (1/?_a?) / (1/?_v? + 1/?_a?)          note w_v + w_a = 1

estimate  ? = w_v?x_v + w_a?x_a

variance  1/?_?? = 1/?_v? + 1/?_a?        ?  ?_? < min(?_v, ?_a)
```

**Two consequences, and both are results the lecture reports separately:**

1. **Ventriloquism.** If `?_v? ? ?_a?` then `w_v ? 1` and the percept sits
   almost on the visual estimate. This is precisely the notes' sentence, derived
   rather than asserted.
2. **The combined estimate beats either alone** ? `?_?` is smaller than both
   inputs. This is the formal content of *"more powerful than just using the
   most appropriate modality"* from [[multisensory-integration]].

## It also explains inverse effectiveness

The lecture gives [[inverse-effectiveness]] on p4 ? *weak stimuli enhance each
other more strongly than strong stimuli* ? and never connects it to the
integration hypothesis on p2. **They are the same fact.**

A weak stimulus is an unreliable one: large `?`. And the *proportional* gain from
combining is largest exactly when the individual estimates are poor:

```
relative improvement = ?_? / min(?_v, ?_a)

strong, reliable cues   ? one cue is already nearly as good as the pair
                          ? small relative gain  ? subadditive
weak, unreliable cues   ? neither cue is much use alone, together they are
                          ? large relative gain  ? superadditive
```

So **inverse effectiveness is what optimal integration predicts**, and it is
measurable in single SC neurons. That is a genuinely strong result ? a
statistical principle showing up as a firing-rate signature ? and the lecture
has both halves on facing pages without joining them.

## ?and modality appropriateness

See [[modality-appropriateness-hypothesis]]. That hypothesis is the limiting case
of this one when a single cue dominates. The notes call them alternatives; the
second subsumes the first.

## Where the reliabilities come from

The p6 summary supplies the missing piece:

> **Levels of neural activity in the unimodal layer may provide the reliability
> of each modality.**

So the weights are not supplied externally ? activity level *is* the reliability
estimate. A strongly driven unimodal layer means a trustworthy cue. That is what
makes the scheme implementable by a network rather than merely a description of
behaviour.

## Pseudocode

```
function integrate(estimates, variances):
    precisions = [1/v for v in variances]
    total = sum(precisions)
    s_hat = sum(p*x for p, x in zip(precisions, estimates)) / total
    var_hat = 1 / total
    return s_hat, var_hat
```

```
# Reading reliability off activity, per the p6 summary:
function reliability_from_activity(unimodal_layer):
    return peak_activity(unimodal_layer)     # high activity ? low variance
```

## Unclear in the source

- **No equations at all.** The ML estimator model is named on p5 as the standard
  the ANN is compatible with, and never written.
- **Never linked to [[inverse-effectiveness]]**, despite being its explanation.
- **Never linked to [[modality-appropriateness-hypothesis]]**, despite
  subsuming it.
- The Gaussian assumption is implicit. The [[histogram-based-som]] actually
  represents **full distributions** rather than mean-and-variance, which is more
  general ? but the notes do not remark on the difference.

## Related

[[ventriloquism-effect]] ? [[modality-appropriateness-hypothesis]] ?
[[inverse-effectiveness]] ? [[multisensory-integration]] ?
[[histogram-based-som]] ? [[unity-assumption]] ? [[superior-colliculus]] ?
[[L07-crossmodal-processing]]
