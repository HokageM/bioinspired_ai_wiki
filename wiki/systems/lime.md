---
title: LIME — Local Interpretable Model-Agnostic Explanations
type: system
tags: [explainability, methods]
sources: [L06]
created: 2026-09-21
updated: 2026-09-21
status: developing
---

# LIME — Local Interpretable Model-Agnostic Explanations

Appears on the final page of [[L06-hierarchical-vision]], immediately after
[[dynamic-weight-sharing]] and immediately before the summary.

## The method, as given

- **Explain a classifier locally**, in the neighbourhood of a sample
- **Create a local dataset by perturbing** the sample and dropping
  interpretable units
- **Classify** these perturbed samples
- **Weigh** the perturbations by their similarity to the sample
- **Train an interpretable classifier** with this perturbed data

## The idea

Unpacking the name, which is unusually descriptive:

- **Local** — do not try to explain the whole model. Explain one prediction.
  A decision boundary that is hopelessly complex globally may be nearly linear
  in a small neighbourhood.
- **Interpretable** — the explanation is a model a human can read (a sparse
  linear model, a short rule), not the original network.
- **Model-agnostic** — the procedure only ever *queries* the classifier. It
  never inspects weights, gradients or architecture. It therefore works on
  anything, including a [[convolutional-network]] whose filters are opaque.

**"Interpretable units"** are the human-meaningful pieces the sample is broken
into — superpixels in an image, words in a sentence. Perturbation means
*switching them off* and seeing whether the prediction survives. Whatever you
can remove without changing the answer was not what the classifier was using.

## Pseudocode

```
function lime(classifier, sample, n_perturbations, kernel_width):
    units = segment_into_interpretable_units(sample)   # superpixels / words

    X, y, weights = [], [], []
    for n in 1 .. n_perturbations:
        mask = random_binary_vector(len(units))        # which units to KEEP
        perturbed = reconstruct(sample, units, mask)   # drop the rest

        X.append(mask)                                 # binary feature vector
        y.append(classifier(perturbed))                # query the black box
        weights.append(
            exp(-distance(sample, perturbed)^2 / kernel_width^2)
        )                                              # closer ⇒ counts more

    # fit something a human can read, weighted toward the original sample
    return fit_sparse_linear_model(X, y, sample_weight=weights)
```

```
# Reading the result: the coefficient on unit u is how much the classifier's
# confidence depends on u being present. Large positive ⇒ evidence for the
# class. Large negative ⇒ evidence against.
```

The weighting step is what makes it *local*: perturbations that mangle the
sample beyond recognition still get classified, but they barely count toward
the fitted explanation.

## Why it is in this lecture

The notes draw no connection at all, which is the most conspicuous gap on the
page. The connection is nevertheless plain:

The whole lecture has argued that [[convolutional-network]]s inherit their
structure from the visual cortex, and that the early layers compute
interpretable things — edges, via kernels like `(-1 -1 -1 / 2 2 2 / -1 -1 -1)`.
But that interpretability evaporates with depth. Nobody can say what filter 47
of layer 12 detects. LIME is the tool you reach for when the biological story
stops explaining what the network actually did.

There is a second, sharper link the lecture also does not make: LIME's
perturbation strategy — **drop a unit and see if the answer changes** — is
methodologically identical to the **lesion studies** cited two pages earlier as
the evidence for [[two-visual-streams]]. Ablate a part, observe the deficit,
infer the function. The same epistemology, applied to a network instead of a
brain.

## Unclear in the source

- **No motivation and no link** to hierarchical vision, despite the obvious one.
- **No authors, no date, no citation.**
- **"Interpretable units"** is not defined or exemplified.
- **"Similarity"** is invoked for the weighting step but no metric is given.
- **"Interpretable classifier"** is not specified — linear model, decision tree,
  rule list? Unstated.
- **Nothing about limitations**, which for LIME are substantial. `[external]`
  Explanations can be unstable across runs because the perturbations are random.

## See also

[[convolutional-network]] · [[two-visual-streams]] · [[receptive-field]] ·
[[neural-similarity-and-dot-product]] · [[local-vs-distributed-representation]]
· [[L06-hierarchical-vision]]
