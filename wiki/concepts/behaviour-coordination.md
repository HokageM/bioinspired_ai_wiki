---
title: Behaviour coordination
type: concept
sources: [L08]
tags: [robotics, agents, architecture]
updated: 2026-09-21
---

# Behaviour coordination

Given several behaviours all producing a response at once, **which one wins ? or
how are they mixed?** This is the function `C` in
[[L08-behaviour-based-robotics]]'s formalism:

- Stimuli `S = [s_1, ?, s_n]`
- Behaviours `B = [b_1, ?, b_n]`
- Gains `G = [g_1, ?, g_n]`
- Response of behaviour `i`: `r_i = b_i(s_i)` ? a **vector**: magnitude and angle
- **Overall response `? = C(G * B(S))`**, where `C` **selects / combines outputs
  to produce a single response vector**

Every architecture in the lecture is a choice of `C`.

## Competitive ? one behaviour wins

| Scheme | Rule | Diagram |
|---|---|---|
| **Subsumption** | **higher-level behaviours can overrule lower-level behaviour output** | layers, high ? low, with suppression nodes `S` |
| **Action selection** | **selection through a criterion** (e.g. signal strength) | `MAX(B1, B2, B3)` |
| **Voting** | **behaviours vote for action response `R`** | `Max(R1, R2, R3)` |

## Cooperative ? all behaviours contribute

[[motor-schema]]s take the other branch:

> **`R = ? (G_i ? R_i)`** ? each response `R_i` weighted by gain factor `G_i`

> **Each schema can be defined independently ? schemas are combined in linear
> combination to single output vector.**

## Pseudocode

```
def C_subsumption(R, priority):        # priority ordered high -> low
    for i in priority:
        if active(R[i]):
            return R[i]                # suppresses everything below
    return REST

def C_action_selection(R):
    return R[argmax(magnitude(r) for r in R)]

def C_voting(R, actions):
    tally = {a: 0 for a in actions}
    for r in R:
        tally[vote_of(r)] += 1         # how a behaviour votes is not specified
    return argmax(tally)

def C_motor_schema(G, R):
    return sum(G[i] * R[i] for i in range(len(R)))     # vector sum
```

## The trade-off

| | Competitive | Cooperative |
|---|---|---|
| Output | one behaviour's vector, unmodified | a blend |
| Guarantees | the winner's intent is executed exactly | none ? the sum may satisfy nobody |
| Failure mode | dithering between winners | [[local-minima-problem|local minima and cycling]] |
| Adding a behaviour | needs a place in the priority order | just add a term |

Competitive coordination never produces an action no behaviour asked for.
Cooperative coordination routinely does ? the vector sum of *go left* and *go
right* is *go straight ahead, into the obstacle*. That is precisely the local
minimum.

> [!note] This is L07's fusion fork, on the output side
> [[fusion-strategies]] classified ways of combining two **sensory** streams and
> listed *multiplication, sum, function* as the join operators; action selection
> is max-fusion and motor schemas are sum-fusion. The module has now met the
> same fork twice ? once for perception, once for action ? and named it
> differently both times.
>
> The [[gated-multimodal-unit]] suggests what is missing from both: a **learned**
> `C`. Every coordination scheme in L08 has `G` fixed by hand.

## See also

- [[subsumption-architecture]] ? [[motor-schema]] ? [[potential-field-navigation]]
- [[reactive-agent]] ? why coordination is the whole design problem
