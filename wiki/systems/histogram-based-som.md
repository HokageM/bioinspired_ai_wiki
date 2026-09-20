---
title: Histogram-based SOM
type: system
tags: [multimodal, self-organisation, statistics]
sources: [L07]
created: 2026-09-21
updated: 2026-09-21
status: solid
---

# Histogram-based SOM

The model at the centre of [[L07-crossmodal-processing]] ? a
[[self-organising-map]] modified so that each unit stores a **distribution**
rather than a point, making it a statistical integrator as well as a clustering
map. Described in the notes as a *novel SOM with probabilities*.

## The architecture it sits in

```
?? SC ??????????????????????????????????   ??????????????????????????
?  Superficial layers:                 ?   ? Inferior Colliculus:   ?
?  visual localisation          ? x_v  ?   ? auditory loc.   ? x_a  ?
?                                 ?    ??????????????????????????????
?                                 ?    ?
?  Deeper integration layer:           ?
?  SOM(x_v, x_a)                ? x_s  ?
????????????????????????????????????????
```

> **The SOM learns which representations cluster together. It could learn map
> registration.**

## What is changed from a standard SOM

| | [[self-organising-map]] (L04) | Histogram-based SOM (L07) |
|---|---|---|
| Unit stores | a single prototype vector `w` | a **histogram per input dimension** |
| Output | distance to BMU | **likelihood of the input given the histograms** |
| Update | move prototype toward input | **update histograms**, not single-valued prototypes |
| Interpretation | a point in feature space | a **PDF** ? population-coded probability density function |

Intermediate steps: **divisive normalisation** is applied, and the output is a
**population-coded probability density function**.

The consequence is that the map represents **uncertainty**, not just location. A
broad histogram is an unreliable estimate; a sharp one is a confident estimate.
That is exactly the quantity [[optimal-cue-integration]] needs and that a
standard SOM cannot express.

> **? The ANN model combines self-organisation and statistical integration.**
> NN integrates, compatible with the **ML estimator model**.

## Why this matters

It resolves a tension the module has been carrying. [[self-organising-map]] is
biologically attractive ? local, unsupervised, competitive ? but it is a
clustering algorithm, and clustering is not integration. By storing histograms
instead of prototypes, the same local competitive learning now implements
something with a principled statistical reading. The biology and the maths stop
pulling in different directions.

## The validation ? and why it is the module's best

> **The model reproduces the same phenomena as the biological SC:**
> depression, spatial principle; enhancement, inverse effectiveness; MSI.

This is a different and stronger kind of claim than the module usually makes. The
model is not declared brain-like because it has layers, or because its units are
called neurons. It is checked against **measured empirical signatures** and shown
to exhibit them.

And the signatures are non-trivial. [[inverse-effectiveness]] in particular is a
subtle, counter-intuitive, quantitative effect ? a model could easily be a fine
integrator and still fail to show it. Reproducing it is evidence the mechanism is
right, not just the vocabulary. See [[ann-brain-correspondence]].

## Pseudocode

```
# Each unit u holds, for each input dimension d, a histogram over values.
function likelihood(unit, x):
    L = 1
    for d in dimensions:
        L *= unit.hist[d][ bin_of(x[d]) ]      # P(x_d | unit)
    return L

function forward(x_v, x_a):
    x = concat(x_v, x_a)
    scores = [ likelihood(u, x) for u in units ]
    return divisive_normalise(scores)          # ? population-coded PDF

function divisive_normalise(scores):
    return [ s / (sum(scores) + eps) for s in scores ]
```

```
function train(inputs, eta, neighbourhood):
    for x in inputs:
        bmu = argmax over u of likelihood(u, x)        # cf. argmin distance
        for u in neighbourhood(bmu):
            h = h_neighbourhood(u, bmu)
            for d in dimensions:
                b = bin_of(x[d])
                u.hist[d][b] += eta * h                # accumulate evidence
                u.hist[d] = normalise(u.hist[d])
```

```
# Where the SC phenomena come from, mechanically:
#
#   enhancement  : x_v and x_a favour the SAME unit ? likelihoods multiply
#                  ? that unit's normalised score rises sharply
#   depression   : they favour DIFFERENT units ? divisive normalisation
#                  splits the mass ? both scores fall below unimodal
#   inverse eff. : broad (weak/uncertain) histograms have more to gain from
#                  multiplication than sharp ones ? superadditivity at low
#                  intensity
#
# Note that all three fall out of "multiply the likelihoods, then normalise".
# The notes report the phenomena but do not derive them.
```

## Unclear in the source

- **Divisive normalisation is named, not defined.**
- **Bin count, histogram initialisation and the update rule** are not specified ?
  the diagram shows histograms being updated but gives no equation.
- **No results, no dataset, no task.** The claim that the model reproduces the SC
  phenomena is stated without any figure or number.
- **"It could learn map registration"** ? raised and never pursued, though it is
  arguably the most important thing such a model could do.
- Whether the histograms are **joint or per-dimension** matters a great deal
  (per-dimension assumes independence across modalities, which is precisely what
  integration is supposed to exploit). The diagram says *histograms for each
  input dimension*, implying independence. Not discussed.

## See also

[[self-organising-map]] ? [[superior-colliculus]] ? [[optimal-cue-integration]]
? [[inverse-effectiveness]] ? [[spatial-principle]] ?
[[multisensory-integration]] ? [[local-vs-distributed-representation]] ?
[[ann-brain-correspondence]] ? [[cross-modal-stimuli-prediction]] ?
[[L07-crossmodal-processing]]
