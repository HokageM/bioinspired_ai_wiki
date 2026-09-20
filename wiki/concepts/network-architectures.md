---
title: Network Architectures
type: concept
tags: [neural-networks, foundations]
sources: [L02, L03, L04, L05, L06, L07]
created: 2026-09-20
updated: 2026-09-21
status: developing
---

# Network Architectures

The four connection topologies sketched in the lecture (L02, p4).

## Biological origin

Follows directly from L02's claim that neurons are very similar across species
and that **development in the brain is how neurons are interconnected**
(L02, p1). If the units are generic, topology is where the capability lives —
which is what makes this list more than bookkeeping.

## The four (L02, p4)

| Architecture | Structure |
|---|---|
| **Feed-forward** | Input layer → output layer, one direction, no cycles |
| **Feed-forward multilayer** | As above with one or more intermediate layers |
| **Recurrent** | Contains cycles — a unit's output can reach its own input |
| **Fully connected** | Every unit connected to every other |

## Computational form

```text
# FEED-FORWARD: evaluate once, in layer order. No state.
y = x
for layer in layers:
    y = phi(layer.W @ y - layer.theta)
return y

# RECURRENT: state persists; must be evaluated over time.
a = zeros(N)
for n in 0 .. T:
    a = update(a, x[n])          # depends on a's own previous value
    y[n] = phi(a - theta)
return y                         # a sequence, not a single vector
```

The difference that matters is the second line of each block: a feed-forward net
has no variable that survives between evaluations, so it cannot represent
*when*. A recurrent net does. The lecture makes this explicit on the same page:
a **feedback loop between the output at time $t_0$ and the input at time $t_1$
provides a temporal delay of the signal into the future** (L02, p4) — this is
how temporal dependencies (e.g. speech, language) are realised in an otherwise
static network. That feedback loop is the recurrent weight $\mu_i$ of the
[[discrete-dynamic-neuron]].

## Where it appears in the module

- [[L02-spiking-neural-networks]] — page 4, immediately before dynamic neuron
  models. The four are sketched with no discussion of purpose.
- [[L03-computational-neural-networks]] — fills in the *why* for two of them:
  feed-forward multilayer is motivated by the [[xor-problem]], and recurrent by
  variable-length sequences. L03 also names **convolutional networks** as a
  structure L02 does not list.

## Which page develops which

| L02 architecture | Developed in |
|---|---|
| Feed-forward | [[mcculloch-pitts-neuron]] |
| Feed-forward multilayer | [[multi-layer-perceptron]] |
| Recurrent | [[recurrent-neural-network]], [[simple-recurrent-network]], [[gated-recurrent-network]] |
| Fully connected | *nothing yet* |
| — (added in L03) | [[convolutional-network]] |

## See also

- [[discrete-dynamic-neuron]] — the recurrent architecture worked out as a
  single unit with a self-loop.
- [[continuous-dynamic-neuron]] — the same in continuous time.
- [[linear-separability]] — the reason multilayer architectures are needed;
  stated in L03, not L02.
- [[mcculloch-pitts-neuron]] — the unit these are built from.

## Additions from L04

Two architecture families arrive that none of L02's four diagrams cover:

- **Competitive / topological** — [[self-organising-map]]. Units are arranged on
  a *lattice*, and the lattice distance (not the weights) decides who learns.
  This is the first architecture in the module where **geometry of the layer
  itself** is functional.
- **Attention-based** — [[gpt]], explicitly *attention instead of recurrence*.
  This directly answers the gap noted below.

Also: [[multi-layer-associator]] is a **bidirectional chain**, run forwards to
produce and backwards to recognise. That is neither feedforward nor recurrent in
L02's sense.

| Architecture | Introduced | Signal flow |
|---|---|---|
| Feedforward | L02 | one way |
| Recurrent | L02 | self-loops in time |
| Fully connected | L02 | *still undeveloped* |
| Competitive/topological | **L04** | lateral, on a lattice |
| Bidirectional associator | **L04** | both ways through a chain |
| Attention | **L04** | all-to-all, no recurrence |

## Additions from L05

L05 adds an architectural pattern rather than a new connectivity:
**the hybrid pipeline**, in which stages of different kinds are chained.

```
  spiking stage  →  rate-coded stage  →  algorithmic stage  →  actuator
```

Both L05 systems have this shape:

| System | Stage 1 | Stage 2 | Stage 3 |
|---|---|---|---|
| [[hybrid-acoustic-tracking]] | algorithmic correlator | [[simple-recurrent-network]] | motor control |
| [[hybrid-spiking-localisation-network]] | spiking MSO/LSO/IC | feed-forward classifier | motor control |

The interesting point is that **the coding scheme changes between stages**.
Nothing before L05 mixed spiking and rate-coded units in one system; L02 treated
them as rival traditions ([[neural-coding]]). Here they are consecutive layers
of the same pipeline, with the spike-timing work done at the front end and the
result handed on as ordinary activations.

See [[hybrid-architecture]] for the design argument behind the seams.

## L06 adds the convolutional and locally connected families

L06 introduces two more architecture classes, and makes their **difference**
the subject of the lecture's argument rather than a passing detail.

| Architecture | Each unit reads | Weights | Parameters |
|---|---|---|---|
| Fully connected | all of the input | all distinct | `n_in × n_out` |
| **[[locally-connected-network]]** | a local patch | **all distinct** | `r × N` |
| **[[convolutional-network]]** | a local patch | **shared across positions** | `r` |

These two differ by a single index — `W[i][j]` versus `w[j]` — and that index
carries the whole biological-plausibility argument of L06:

> **CNNs require [[weight-sharing]], which real neurons cannot do.**
> **Locally connected networks do not share weights but perform worse than CNNs
> on image classification tasks.**

So for the first time the module presents an architectural choice as a **trade
between performance and plausibility**, rather than as a menu. And it proposes
repairs — [[data-augmentation]] and [[dynamic-weight-sharing]] — that keep the
plausible topology and try to make the sharing *emerge*.

### Depth as alternating function

L06 also adds a structural idea absent from L02's four diagrams: layers that
**alternate in kind**, each type doing a different job.

```
  detect  →  discard position  →  detect  →  discard position  →  classify
   conv         pooling            conv        pooling            dense
   S-cell       C-cell             S-cell      C-cell
   simple       complex            simple      complex
```

The same alternation appears at all three levels — cortex
([[simple-complex-hypercomplex-cells]]), the [[neocognitron]], and LeNet-5. This
is a different axis from L02's feed-forward/recurrent/fully-connected taxonomy,
which classified networks by *connectivity*; here they are classified by
*what each layer is for*.

Compare the **hybrid pipeline** above: L05 chained stages with different
**coding schemes**, L06 chains layers with different **functions**. Both are
arguments that a useful system is heterogeneous.

## Open questions / gaps

- The four are sketched in L02 as diagrams with almost no text.
- **"Fully connected" is still undeveloped** — drawn as a small ring of mutually
  connected units, which looks like a Hopfield-style network, but never named
  or revisited in L02 or L03.
- ~~No mention of attention-based architectures anywhere so far.~~
  **Resolved in L04** — [[gpt]] is *attention instead of recurrence*, though
  attention itself is still never defined.
- L06 gives no training procedure for the [[convolutional-network]] — the
  forward pass is fully specified and learning is not mentioned at all.


## L07: fusion as an architectural axis

[[L07-crossmodal-processing]] adds a classification orthogonal to every previous
one. L02 classified by **connectivity**, L06 by **what each layer is for**; L07
classifies by **where two input streams meet**. See [[fusion-strategies]]:

| Strategy | Meeting point |
|---|---|
| **Early fusion** | at the input ? a concatenated vector |
| **Intermediate fusion** | after separate representations are learned; *in one layer or gradually* |
| **Late fusion** | at the output ? combining sub-model decisions |

Plus four *simple types* for the join itself: concatenation, multiplication, sum,
function. The [[gated-multimodal-unit]] is the *function* case.

And the [[cortico-collicular-architecture]] adds something the taxonomy has no
name for: **fusion that feeds back to modulate an earlier fusion**. See
[[top-down-modulation]].
