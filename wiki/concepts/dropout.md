---
title: Dropout
type: concept
tags: [learning, generalisation, neural-networks]
sources: [L03]
created: 2026-09-20
updated: 2026-09-20
status: developing
---

# Dropout

> **Randomly set ≈50% of all neuron activations to 0 during training.**
> (L03, p4)

## Biological origin

Not claimed in the notes, though it is suggestive: it is the training-time
analogue of the **robustness to damage or noise** that
[[local-vs-distributed-representation]] attributes to distributed codes in L02.
Dropout works by *forcing* the network into a distributed representation — no
unit can be relied upon, so no unit can become a grandmother cell. That
connection is `[external]`; the lecture lists dropout as a bare technique.

## Computational form

```text
# dropout, training time
for each forward pass:
    for each hidden unit i:
        keep[i] = 1 with probability p else 0      # p ~ 0.5 per L03
        a[i] = a[i] * keep[i]                      # zeroed units contribute
                                                   # nothing, and receive no
                                                   # gradient this pass

# a NEW random mask is drawn every pass — the point is that the network never
# sees the same architecture twice.

# INFERENCE time: no dropout. All units active.
# [external] activations must then be scaled by p (or, equivalently, divided by
# p during training) so the expected input to each unit is unchanged. L03 does
# not mention this, but without it the network is miscalibrated at test time.
```

Why it helps: a unit cannot come to depend on any specific other unit being
present, so the network cannot build fragile co-adapted chains. It is also
effectively training an ensemble of subnetworks that share weights.

## Where it appears in the module

- [[L03-computational-neural-networks]] — page 4, last of the five remedies for
  overfitting.

## See also

- [[overfitting-and-underfitting]] — the problem it addresses.
- [[regularisation]] — the penalty-based alternative on the same list.
- [[local-vs-distributed-representation]] — what dropout forces the network
  towards.

## Open questions / gaps

- **The train/test asymmetry is not mentioned** — arguably the single most
  important implementation detail, since omitting the rescaling breaks
  inference. Marked `[external]` above.
- No justification is given for ≈50%, and no guidance on which layers to apply
  it to.
- The notes do not connect dropout to the L02 material on distributed codes,
  although the argument is the same.
