---
title: Perceptron Convergence Theorem
type: concept
tags: [learning, supervised, foundations, geometry]
sources: [L03]
created: 2026-09-20
updated: 2026-09-20
status: solid
---

# Perceptron Convergence Theorem

> **Given linearly separable data, the perceptron will find a separating
> hyperplane in a finite amount of steps.** (L03, p2)

## Biological origin

None — this is a mathematical guarantee about the
[[perceptron-learning-rule]]. It is included because it is the reason the
perceptron was taken seriously, and because its precondition is exactly what
the [[xor-problem]] violates.

## What it does and does not promise

| Promised | Not promised |
|---|---|
| Termination in finitely many steps | Any particular number of steps |
| *A* separating hyperplane | The *best* separating hyperplane (no margin guarantee) |
| Correctness on the training data | Generalisation — see [[overfitting-and-underfitting]] |
| — | Anything at all if the data is **not** linearly separable |

The final row is the load-bearing one. If the data is not linearly separable the
rule does not converge — it cycles indefinitely, since some example is always
misclassified and always produces an update. The theorem's precondition is
therefore also a diagnosis of its failure mode.

## Computational form

```text
# the theorem, as a contract on [[perceptron-learning-rule]]
#
# PRECONDITION:  exists w* such that for all (x, t) in D:
#                    sign(dot(w*, x)) == t          # D is linearly separable
#
# GUARANTEE:     the loop
#                    while any example misclassified:
#                        w = w + eta * (t - y) * x
#                terminates after a FINITE number of updates.
#
# IF THE PRECONDITION FAILS: the loop does not terminate. There is no
# separating hyperplane to find, so some example is always wrong, so an
# update always fires. The algorithm has no way to report failure.
```

Practically this means a run that never terminates is evidence that the data is
not linearly separable — which is how the [[xor-problem]] shows up in practice.

## Where it appears in the module

- [[L03-computational-neural-networks]] — page 2, immediately before the Boolean
  function limitations.

## See also

- [[perceptron-learning-rule]] — the algorithm this is about.
- [[linear-separability]] — the precondition.
- [[xor-problem]] — the counterexample, given on the very next line of the notes.
- [[multi-layer-perceptron]] — what you use when the precondition fails. Note
  that **no equivalent convergence theorem exists for it** —
  [[backpropagation]] carries no such guarantee.

## Open questions / gaps

- No proof, no bound on the number of steps, and no reference.
- The notes do not state the contrapositive — that non-separable data causes
  non-termination — even though the [[xor-problem]] appears on the next line.
- Nothing is said about what the theorem means for *multi-layer* networks; the
  absence of a guarantee there is arguably the more important fact.
