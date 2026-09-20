---
type: concept
tags: [multimodal, architecture, neural-networks]
sources: [L07, L08]
status: solid
---

# Fusion strategies

How and where two modalities get combined inside a network, from
[[L07-crossmodal-processing]] pp2?3.

## Simple types ? *how* two vectors combine

| Type | Operation | Note |
|---|---|---|
| **Concatenation** | `[a ; b]` | keeps everything; lets the next layer decide |
| **Multiplication** | `a ? b` | gating ? either input can veto |
| **Sum** | `a + b` | requires matching dimensions and comparable scales |
| **Function** | `f(a, b)` | anything learned, e.g. [[gated-multimodal-unit]] |

Concatenation and sum are linear and lossless-ish; multiplication is the only one
of the four that can express *"only if both"*, which is why gating architectures
are built from it.

## Integration paths ? *where* it happens

| Strategy | Description (verbatim) |
|---|---|
| **a) Early fusion** | *takes as input a concatenated vector* |
| **b) Intermediate fusion** | *first representations of the data are learned, afterwards the modalities are fused. This can occur in one layer or gradually* |
| **c) Late fusion** | *combines decisions by sub-models for each modality* |

```
early:         [v ; a] ? ???????? net ???????? ? decision

intermediate:  v ? net_v ??
                          ?? fuse ? net ? decision
               a ? net_a ??

late:          v ? net_v ? decision_v ??
                                       ?? combine ? decision
               a ? net_a ? decision_a ??
```

## The trade

The notes give the taxonomy without the analysis. The trade is:

- **Early** fusion can learn arbitrary interactions between raw features, but
  must do so from scratch, and one missing modality breaks the input vector.
- **Late** fusion is robust ? each sub-model works alone, and a missing modality
  just drops a vote ? but it can only combine *conclusions*, so any interaction
  that is not visible in the separate decisions is lost forever.
- **Intermediate** fusion is the compromise, and *"gradually"* is the
  interesting word: fusion need not happen at one depth.

**The brain does intermediate-and-gradual.** That is the point of the
[[superior-colliculus]] having separate unimodal layers feeding a deeper
integration layer, and of the [[cortico-collicular-architecture]] fusing again at
cortical level with feedback down. The lecture presents the NN taxonomy on p2?3
and the biology on p3?6 without ever placing the biology in the taxonomy.

## Pseudocode

```
function early_fusion(v, a):
    return net(concat(v, a))

function intermediate_fusion(v, a):
    return net(fuse(net_v(v), net_a(a)))       # fuse ? {concat, sum, ?, GMU}

function late_fusion(v, a):
    return combine(net_v(v), net_a(a))         # combine over DECISIONS
                                               # e.g. weighted vote
```

```
# Why late fusion cannot be repaired by a better combiner:
#   if net_v collapses "red square" and "red circle" to the same decision,
#   no downstream combiner can recover the distinction from a.
#   Information destroyed before the fusion point is gone.
```

## Related

[[multisensory-integration]] ? [[gated-multimodal-unit]] ?
[[cortico-collicular-architecture]] ? [[superior-colliculus]] ?
[[network-architectures]] ? [[hybrid-architecture]] ?
[[cross-modal-stimuli-prediction]] ? [[L07-crossmodal-processing]]


## The same fork, one lecture later, for motor output (L08)

[[L08-behaviour-based-robotics]] faces the identical decision on the way **out**
of the agent rather than in, and arrives at the same two options under different
names. See [[behaviour-coordination]].

| L07 (sensory fusion) | L08 (behaviour coordination) |
|---|---|
| join by **sum** | [[motor-schema]]s: `R = ?(G_i ? R_i)` |
| join by **max** | action selection: `MAX(B1,B2,B3)`; voting |
| join by **function** ([[gated-multimodal-unit]]) | ? **no equivalent** |
| late fusion (combine decisions) | [[subsumption-architecture]] |

Two observations the module does not make:

1. **The trade-off is the same one.** Summing is smooth and can produce an
   output no contributor wanted ? which on the motor side is exactly the
   [[local-minima-problem]]. Taking the max is discontinuous but always returns
   something a contributor actually proposed.
2. **Only the sensory side has a learned join.** The GMU learns `?`; every gain
   `G_i` in L08 is set by hand. The obvious missing system is a gated motor
   schema, and nothing in the module rules it out.
