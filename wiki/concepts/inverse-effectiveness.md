---
type: concept
tags: [multimodal, neuroscience, statistics]
sources: [L07]
status: solid
---

# Inverse effectiveness

> **Weak stimuli enhance each other more strongly than strong stimuli.**
> ? [[L07-crossmodal-processing]], p4

The second measured signature of [[superior-colliculus]] neurons, after the
[[spatial-principle]].

| Stimulus intensity | Combined response vs. sum of parts |
|---|---|
| **strong** | **subadditive** ? less than the sum |
| **weak** | **superadditive** ? **more** than the sum |

The bar charts compare the measured bimodal response against a **theoretical**
bar, which is the arithmetic sum of the two unimodal responses. Superadditivity
means the real neuron beats that sum.

## Why it happens

The lecture does not say. But it gave the answer two pages earlier, under the
integration hypothesis for the [[ventriloquism-effect]], and never joined them.

**Inverse effectiveness is what [[optimal-cue-integration]] predicts.**

A weak stimulus is an *unreliable* one ? large variance. The proportional benefit
of combining two estimates is greatest exactly when neither is much good alone:

```
combined precision  1/?? = 1/?_v? + 1/?_a?

two strong cues : either one is already nearly as good as the pair
                  ? small relative gain  ? subadditive
two weak cues   : neither is usable alone, together they are
                  ? large relative gain  ? superadditive
```

This is a genuinely striking result and the wiki flags it as the lecture's best
unmade point: **a statistical principle about estimator variance shows up as a
firing-rate signature in a single midbrain neuron.** The notes have both halves
on facing pages.

It also explains *why* a brain would bother with MSI at all. If integration only
helped when signals were already strong, it would be a luxury. Because it helps
most when signals are weak, it is exactly the mechanism you want in fog, in
darkness, at distance ? the conditions where orienting correctly matters most.

## Pseudocode

```
function response(v, a, colocated):
    if not colocated:
        return depression(v, a)          # see spatial-principle
    # superadditive at low intensity, saturating at high intensity
    return saturate(v + a + interaction(v, a))

function interaction(v, a):
    # large when both are weak, vanishing when either is strong
    return gain * (1 - v) * (1 - a)      # [external] ? one functional form
                                         # that produces the observed curve
```

```
# The measured quantity in the bar charts:
additivity_index = response(v, a) / (response(v, 0) + response(0, a))
#   > 1 superadditive   (weak stimuli)
#   < 1 subadditive     (strong stimuli)
```

## As a design principle

The p6 summary lists inverse effectiveness as one of the model's validated
behaviours, alongside the spatial principle ? see [[histogram-based-som]]. The
[[ann-brain-correspondence]] point is that these are **testable signatures**: a
model either shows them or does not. That is a far stronger form of
correspondence than the module's earlier claims of resemblance.

## Unclear in the source

- **Never connected to [[optimal-cue-integration]]**, though that is its
  explanation.
- **No functional form**, no numbers, no threshold between the weak and strong
  regimes.
- **Saturation is the obvious confound** and is not addressed: a strong stimulus
  may appear subadditive simply because the neuron's firing rate is near ceiling.
  `[external]` Distinguishing genuine subadditivity from saturation is a known
  methodological issue.

## Related

[[spatial-principle]] ? [[superior-colliculus]] ? [[optimal-cue-integration]] ?
[[multisensory-integration]] ? [[histogram-based-som]] ?
[[ann-brain-correspondence]] ? [[rate-coding]] ? [[L07-crossmodal-processing]]
