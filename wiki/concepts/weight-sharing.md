---
type: concept
tags: [convolution, plausibility, architecture]
sources: [L06]
status: solid
---

# Weight sharing

The mechanism that makes a [[convolutional-network]] work, and — according to
[[L06-hierarchical-vision]] — the mechanism that makes it **biologically
implausible**.

## What it is

The same filter `w` is applied at every position of the input:

```
z_i = Σ_{j=0}^{r−1}  w_j · x_{i+j}          # same w for every i
```

The notes' margin gloss: *to detect common local features across an image by
applying the same weight.*

**The payoff is threefold:**

1. **Parameter count.** One filter of size `r` covers an input of any length.
   A [[locally-connected-network]] needs `r` weights *per position*.
2. **Translation equivariance.** A feature learned at one position is detected
   at every position, for free. No need to see the same object in every corner.
3. **Statistical efficiency.** Every patch in every image is a training example
   for the same filter, so the filter sees `N` times more data.

## The problem

> **CNNs require weight sharing, which real neurons cannot do.**

This is the sharpest sentence in the module. There is no mechanism by which two
synapses in different cortical columns, on different neurons, could be held
numerically identical. Weight sharing is an *implementation convenience of the
array*, not a fact about tissue. The backward pass sums the gradient from every
position into one shared parameter — a global operation, requiring information
to travel between physically distant synapses that have no connection.

Compare the symmetric complaint about [[backpropagation]] on
[[ann-brain-correspondence]]: both are objections of the same form, that the
algorithm requires information at a synapse that could not physically be there.

## The consequence

> **Locally connected networks do not share weights but perform worse than CNNs
> on image classification tasks.**

```
   locally connected              convolutional
      w_1 ≠ w_4                      w_1 = w_4
   plausible, worse               implausible, better
```

So the field faces an explicit trade, and L06 poses it as a research question:

> **How to bridge the gap between the biologically plausible locally connected
> network and the well-performing but less plausible CNN?**

**Two mechanisms** are offered:

1. [[data-augmentation]] via multiple image translations
2. [[dynamic-weight-sharing]]

Both attack the same point. Neither *imposes* `w_1 = w_4` by fiat; both try to
make it **emerge** — `w_1 ≈ w_4` — from a local process.

## Pseudocode — the three regimes

```
# 1. Convolutional: one weight vector, used everywhere. Enforced identity.
for i in patches:
    z[i] = dot(w, x[i : i+r])

# 2. Locally connected: a private weight vector per position. No constraint.
for i in patches:
    z[i] = dot(w[i], x[i : i+r])

# 3. Dynamic weight sharing: private weights, pulled together by a local rule.
for i in patches:
    z[i] = dot(w[i], x[i : i+r])
sleep_phase()            # see dynamic-weight-sharing — drives w[i] → w[j]
```

Regime 3 is the interesting one: it keeps the plausible architecture of 2 and
tries to recover the behaviour of 1.

## Why this matters to the wiki

This is the first place in the module where **biological plausibility is treated
as a constraint to be satisfied rather than a badge to be claimed.** L03
asserted that backpropagation corresponds to synaptic plasticity and moved on.
L06 identifies a specific requirement, says flatly that neurons cannot meet it,
and then does engineering work to get around it — with honest reporting of how
well that works (*only small performance improvement*).

See [[ann-brain-correspondence]], where this is recorded as raising the
module's standard.

## Related

[[convolutional-network]] · [[locally-connected-network]] ·
[[dynamic-weight-sharing]] · [[data-augmentation]] · [[backpropagation]] ·
[[ann-brain-correspondence]] · [[network-architectures]] ·
[[L06-hierarchical-vision]]
