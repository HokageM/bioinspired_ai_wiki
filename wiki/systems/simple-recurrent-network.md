---
title: Simple Recurrent Network (SRN)
type: system
tags: [recurrent, neural-networks, supervised]
sources: [L03, L05, L12]
created: 2026-09-20
updated: 2026-09-20
status: solid
---

# Simple Recurrent Network (SRN)

The basic trainable [[recurrent-neural-network]]: an input layer, a hidden
layer, an output layer, and a **context** layer holding the previous hidden
state (L03, p5).

## Biological origin

The notes make a direct claim worth flagging:

> **Biologically more plausible than an MLP.** (L03, p5)

This is the module quietly conceding what [[ann-brain-correspondence]] asserts
away — that a plain [[multi-layer-perceptron]] is *not* especially plausible.
The plausibility argument for recurrence is that real cortical circuits are
massively recurrent, whereas strictly layered feed-forward structure is rare.
Note the irony: the SRN is *more* plausible architecturally but is trained by
backpropagation through time, which is *less* plausible than ordinary
backpropagation, since it requires storing and replaying a history.

## Architecture (L03, p5)

```
   Output  [ o o ... o ]
              ↑
   Hidden  [ o o ... o ] ─────┐   h_{t+1} = f(h, x)
            ↑          ↑      │
   Input [ o..o ]   Context ◄─┘   (context = hidden state at t-1)
                    [ o..o ]
```

## Training: backpropagation through time (L03, p5)

> **Backpropagation realised by unrolling recurrent connections, i.e.
> backpropagation through time.**

```text
# BPTT — unroll the cycle into a deep feed-forward net, then use [[backpropagation]]
#
#   recurrent view:          unrolled view:
#        ┌──┐
#        ▼  │                x1 -> [h] -> [h] -> [h] -> y
#   x -> [h]┘                       t=1    t=2    t=3
#                             (the SAME weights at every step)

def bptt(sequence, targets, W_x, W_h, W_y, eta):

    # --- forward: keep every intermediate state ---
    h = [h_0]
    for t in 1 .. T:
        h.append( phi(W_x @ x[t] + W_h @ h[t-1]) )
        y[t] = W_y @ h[t]

    # --- backward: through time, newest first ---
    dh_next = 0
    dW_x = dW_h = dW_y = 0
    for t from T down to 1:
        dy   = targets[t] - y[t]
        dW_y += outer(dy, h[t])

        dh   = transpose(W_y) @ dy + dh_next
        de   = dh * dphi(h[t])

        dW_x += outer(de, x[t])
        dW_h += outer(de, h[t-1])
        dh_next = transpose(W_h) @ de     # <-- W_h multiplied AGAIN, every step
                                          #     T times in total ==>
                                          #     [[vanishing-gradient-problem]]

    # --- one update, shared weights ---
    W_x += eta * dW_x;  W_h += eta * dW_h;  W_y += eta * dW_y
```

The marked line is the whole weakness: $W_h$ enters the gradient once per time
step, so the error signal reaching early steps has been multiplied by $T$
factors. **Prone to the [[vanishing-gradient-problem]]** (L03) — *gradient
becomes small → no update*.

## Where it appears in the module

- [[L03-computational-neural-networks]] — page 5.

## See also

- [[recurrent-neural-network]] — the general idea.
- [[gated-recurrent-network]] — what fixes the gradient problem.
- [[vanishing-gradient-problem]] — the failure mode.
- [[backpropagation]] — the algorithm being unrolled.
- [[discrete-dynamic-neuron]] — L02's single-unit equivalent.
- [[hybrid-acoustic-tracking]] (L05) — the SRN doing a concrete job.

## Application in L05

L05 uses an SRN as the **tracking** stage of [[hybrid-acoustic-tracking]]:
*prediction of sound angle for a moving sound source*, i.e. a predictor for the
next angle in the trajectory of the source.

- **Input:** current sound angle. **Output:** predicted sound angle.
- Layer sizes written on the arrows: `45:30` input→hidden, `30:45`
  hidden→output, `30:30` hidden↔context at **1:1**.
- `t_n` → prediction → second input `t_n+1` → now can predict `t_n+2`.
  ⇒ **two inputs for context needed.**

That last line is the first concrete statement in the module of recurrence's
**warm-up cost**: the context layer is meaningless until it has been filled, so
the network is blind for its first steps. L03 described the copy-back mechanism
but never noted this consequence.

The lecture's justification is also worth recording, because it is an argument
about *when* to use an RNN at all: *neural predicting of source enables a
quicker response and can learn temporal sequences*, while the localisation
itself stays algorithmic because it is already understood. See
[[hybrid-architecture]].

> [!note] The layer sizes are ambiguous
> `45:30`, `30:45` and `30:30 1:1` are written on arrows with no units. The
> reading adopted in the wiki is input 45 → hidden 30 → output 45, with a
> 30-unit context layer copied 1:1 from the hidden layer. Why a single angle
> needs 45 input units is unexplained — 4° bins over 180° would fit, which would
> make the input a place code rather than a scalar.

## Open questions / gaps

- **The "biologically more plausible" claim is asserted without argument** and
  is not reconciled with BPTT's own implausibility.
- No equations are given — the architecture is a diagram and
  $h_{t+1} = f(h, x)$.
- Truncated BPTT is not mentioned, so nothing addresses the cost of unrolling a
  long sequence.
- "Elman network" and "Jordan network" are not used as names.

## L12 ? reading an SRN out as a symbol processor

L12 takes recurrent networks as its main worked example of
[[knowledge-extraction]], asking directly:

> **How to interpret RNN?**

and giving three answers: [[hinton-diagram|weight visualisation]], activation
clustering (dendrograms, PCA), and [[automata-extraction|automaton extraction]].

The premise is that an SRN's **context layer** holds a **state**,
and a system with inputs, states and outputs is a **transducer** ? so an SRN can
be read as a finite state machine over a continuous state space. See
[[transducer-network]] for the syntactic task used throughout, and
[[preference-moore-machine]] for the formalisation.

This retroactively reframes what L03 built. A copy of the hidden layer introduced as an engineering device for sequence memory is, in L12's
reading, the machine's *state register*.
