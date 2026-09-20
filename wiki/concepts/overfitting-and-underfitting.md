---
title: Overfitting and Underfitting
type: concept
tags: [learning, generalisation]
sources: [L03]
created: 2026-09-20
updated: 2026-09-20
status: solid
---

# Overfitting and Underfitting

The **generalisation problem** (L03, p4) — the two ways a trained model fails.

## Biological origin

None stated, though the underlying tension is general to any learner: L01's
*react to something new* ([[intelligent-behaviour]]) is exactly the demand that
a model generalise beyond what it has seen.

## The two failures (L03, p4)

| Failure | Definition in the notes | Sketch |
|---|---|---|
| **Overfitting** | *adapting to noise* | a wiggly curve chasing every point |
| **Underfitting** | *little or no adaptation to structure of data* | a flat line ignoring the points |

On overfitting specifically (L03):

- Training on a **small training set** ⇒ poor performance.
- **Instead of generalising, the network learns the particularities of
  individual samples.**

The diagnosis is about *what* is learned, not how much: the network has capacity
left over after capturing the structure, and spends it memorising noise.

## Remedies for overfitting (L03, p4)

The lecture lists five:

1. **Generate more training data** — more signal to fill the capacity.
2. **Early stopping** — stop before the spare capacity gets used.
3. **[[regularisation]]** — penalise large weights.
4. **Augment the dataset** — add distorted images, add noise, etc.
5. **Apply [[dropout]]** — randomly zero ≈50% of activations during training.

They split into two strategies: **give the model more data** (1, 4) or **give
the model less freedom** (2, 3, 5).

## Computational form

```text
# the shape of the problem
train_error  decreases monotonically with training
val_error    decreases, then TURNS AROUND and increases   <- overfitting begins

# early stopping = detect the turn
best = infinity
for epoch in 1 .. MAX:
    train_one_epoch()
    v = error_on(validation_set)         # data NOT used for the updates
    if v < best:
        best = v
        save_weights()
    else if v > best for PATIENCE consecutive epochs:
        restore_weights()                # go back to the turning point
        break

# underfitting has no turning point: BOTH errors stay high.
# remedy is the opposite — more capacity, longer training, less regularisation.
```

## Where it appears in the module

- [[L03-computational-neural-networks]] — page 4.

## See also

- [[regularisation]], [[dropout]] — two of the remedies.
- [[loss-function]] — what is being minimised, and why minimising it fully is
  the wrong goal.
- [[perceptron-convergence-theorem]] — guarantees fitting the *training* data,
  which this page shows is not the same as succeeding.
- [[batch-vs-online-training]] — the other half of L03's practical training
  material.

## Open questions / gaps

- **No validation set is mentioned anywhere** — yet early stopping is
  meaningless without one, and the notes give no way to detect overfitting. The
  early-stopping loop above marks this as reconstructed.
- No train/test split, no cross-validation, no metrics.
- The notes do not give a remedy for *underfitting*, only for overfitting.
- The "≈50%" dropout figure is asserted with no justification.
