---
title: Gated Recurrent Networks (LSTM / GRU)
type: system
tags: [recurrent, neural-networks, supervised]
sources: [L03, L04, L07, L10]
created: 2026-09-20
updated: 2026-09-21
status: developing
---

# Gated Recurrent Networks (LSTM / GRU)

Recurrent networks in which learned **gates** control what enters, persists in,
and leaves a memory cell (L03, p5).

## Biological origin

Not claimed. Gating is an engineering solution to the
[[vanishing-gradient-problem]], not a copied mechanism.

## The gates (L03, p5)

The lecture draws a cell $C_{t-1}$ with sigmoid-controlled gates. Each gate
outputs a value in $[0, 1]$ — the notes annotate the range explicitly as
*forget ↔ remember* and *forget ↔ add*:

| Gate | The notes' description | Question it answers |
|---|---|---|
| **Input gate** | *what should be added* | How much of the new candidate enters the cell? |
| **Forget gate** | *what should be forgotten* | How much of the existing cell state survives? |
| **Output gate** | — | How much of the cell is exposed as $h_t$? |

And (L03):

- → **Long short term memory (LSTM)**
- → **addresses the vanishing gradient problem**
- → **update gate and reset gate** ⇒ *what information should pass to the output*

## Computational form

```text
# LSTM cell, one time step
#   x[t]     current input
#   h[t-1]   previous output
#   C[t-1]   previous cell state — the long-term memory

f = sigmoid(W_f @ [h[t-1], x[t]] + b_f)     # FORGET gate   in [0,1]
i = sigmoid(W_i @ [h[t-1], x[t]] + b_i)     # INPUT gate    in [0,1]
o = sigmoid(W_o @ [h[t-1], x[t]] + b_o)     # OUTPUT gate   in [0,1]

C_hat = tanh(W_c @ [h[t-1], x[t]] + b_c)    # candidate value to add

C[t] = f * C[t-1]  +  i * C_hat             # <<< THE KEY LINE
h[t] = o * tanh(C[t])                       # what the cell exposes
```

**Why that line fixes the gradient.** In a [[simple-recurrent-network]] the
state is *transformed* every step — `h = phi(W_h @ h + ...)` — so the backward
pass multiplies by $W_h$ and $\phi'$ repeatedly, and the product decays. Here
the cell state is *added to*:

```text
#   C[t] = f * C[t-1] + (something)
#
#   dC[t]/dC[t-1] = f          <- just the forget gate, no weight matrix,
#                                 no squashing derivative
#
#   if the network learns f ~ 1, the gradient passes through UNCHANGED for as
#   many steps as it likes. this is the "constant error carousel".
#   [external] L03 states that LSTM addresses the problem but not this mechanism.
```

The network *learns how long to remember*, because $f$ is itself a function of
the input.

> [!note] LSTM and GRU are conflated in the source
> The notes describe input/forget/output gates — an **LSTM** — and then append
> *"update gate and reset gate"*, which are the gates of a **GRU**, a different
> (simpler) architecture with no separate cell state. Neither "GRU" nor "gated
> recurrent unit" is written. Treat the final line as a separate architecture,
> not as extra LSTM gates.

```text
# GRU, for contrast — 2 gates, no separate cell state
z = sigmoid(W_z @ [h[t-1], x[t]])           # UPDATE gate (merges forget+input)
r = sigmoid(W_r @ [h[t-1], x[t]])           # RESET gate
h_hat = tanh(W @ [r * h[t-1], x[t]])
h[t] = (1 - z) * h[t-1] + z * h_hat         # same additive trick
# [external] — reconstructed from the two gate names the notes give.
```

## Where it appears in the module

- [[L03-computational-neural-networks]] — page 5.

## See also

- [[vanishing-gradient-problem]] — the problem solved.
- [[simple-recurrent-network]] — what it replaces.
- [[recurrent-neural-network]] — the general setting.
- [[activation-function]] — gates are sigmoids precisely because the sigmoid is
  *bounded* in $(0,1)$, making it usable as a proportion.

## Open questions / gaps

- **No equations in the source** — the page is a diagram plus gate names. All
  formulae above are reconstructed and marked.
- **LSTM/GRU conflation** (see note).
- The mechanism by which LSTM addresses the vanishing gradient is asserted, not
  explained.
- Peephole connections, and the question of whether LSTM or GRU is preferable,
  are not discussed.

## Sequel in L04

L04 ends this thread rather than continuing it. [[gpt]] is described as
**attention instead of recurrence** — the gated cell is not improved, it is
removed. The [[vanishing-gradient-problem]] that motivated gating disappears
along with the recurrence, because every position connects to every other in one
step rather than through a chain of `T` multiplications.

The module therefore contains the whole historical arc —
[[simple-recurrent-network]] → gating → attention — but never says so, and never
defines attention.


## The same gate appears across modalities (L07)

[[L07-crossmodal-processing]]'s [[gated-multimodal-unit]] uses the identical
convex-combination gate, on a different axis:

```
GRU  (L03):  h = ? ? h_prev   + (1 ? ?) ? h_candidate      # across TIME
GMU  (L07):  z = ? ? tanh(W_v x_v) + (1 ? ?) ? tanh(W_t x_t)   # across MODALITIES
```

Same arithmetic, same `?`/`(1??)` pairing, same purpose: **a learned decision
about how much to trust each of two sources.** In the GRU the sources are the
past and the present; in the GMU they are vision and audition.

Neither lecture mentions the other. Recorded here because the module has now used
this construction twice without naming it, and it is a genuinely general pattern:
*when a network must combine two candidate representations, make the mixing
weight a learned function of both.*


## L10 ? LSTM as the temporal half of a hybrid

[[cnn-lstm]] states the division of labour more crisply than L03 or L07 did:

> **CNN:** learning of significant visual features ? invariance, spatial
> properties
> **LSTM:** variations in performance ? gesture types, across subjects

So the CNN supplies **invariance in space** ([[weight-sharing]]) and the LSTM
supplies **invariance in time** (a gated state). Neither can supply the other's,
which is precisely why [[multichannel-cnn|MCCNN]]'s 3D kernels failed: a fixed
temporal window is not time-invariance.

### Two configurations, stated for the first time

> **Prediction at the last timestep (many-to-one) vs prediction at each timestep
> (one-to-one).**

| | Frame-level | Sequence-level |
|---|---|---|
| Training | online | mini-batch |
| Sequences | varying length supported | sliced and **padded** to fixed length |
| Output | per frame + post-processing | one per sequence |

> [!warning] Padding destroys the timing, and the timing is the signal
> Sequence-level training requires a fixed length, so real gestures are stretched
> or truncated to get one. For a task whose classes differ in their **temporal**
> profile ? see [[motion-intensity-profile]] ? that is a substantive distortion,
> introduced purely for batching convenience. The source lists it as a neutral
> implementation detail.

### A second way to be recurrent

[[gamma-gwr|Gamma-GWR]] (L10) achieves temporal sensitivity with **no gradient
at all**: each node carries a context vector, the distance function includes it,
and the winner therefore depends on history. It is recurrence implemented inside
[[winner-take-all|competition]] rather than inside [[backpropagation]] ? and it
is unsupervised, which the LSTM is not.

[[reservoir-computing]] is named once in L10's summary as a third option, and
never explained.
