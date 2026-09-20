---
title: Hinton diagram
type: system
sources: [L12]
tags: [explainability, methods, representation]
updated: 2026-09-21
---

# Hinton diagram

**Weight analysis for knowledge extraction.** A grid: one row per goal neuron,
one column per input or hidden unit, each cell a square whose **size** is the
magnitude of the weight and whose **colour** is its sign.

> **Black = negative weights. White = positive weights.**

In the notes the rows are `h1, h2, h3, VG, NG, PG` and the columns are input
symbols `s, n, j, v, a, r, u, d` followed by `h1 h2 h3` and `c1 c2 c3` ? so the
diagram shows a [[transducer-network|recurrent transducer's]] input, hidden and
context weights in one picture.

```
         s   n   j   v   a   r   u   d   h1  h2  h3  c1  c2  c3
   h1   [ ] ??? [ ] ...                                            # ??? negative
   h2   ??? [ ] ...                                                # [ ] positive
   VG   ...
```

## The verdict

> - **Can explain certain phenomena with weight analysis**
> - **Difficult to directly interpret and extract explicit knowledge**
> - **Static representation of weights does not show dynamics of recurrent
>   networks**
> - **Distribution of weights difficult to show for larger networks**

Three objections, and each motivates a different successor:

| Objection | Successor |
|---|---|
| hard to extract explicit knowledge | [[weight-based-transfer]] ? turn weights into rules |
| does not show **dynamics** | [[automata-extraction]] ? read the state transitions |
| does not scale to **larger networks** | [[class-activation-map]], [[layer-wise-relevance-propagation]] ? explain one decision, not the whole model |

> [!note] The scaling objection is the one that generalises
> A Hinton diagram is `|units| ? |units|` squares. At a few dozen units it is a
> picture; at a million it is noise. That is not a defect of the visualisation ?
> it is the statement that **no depiction of all the parameters can be an
> explanation** of a large network.
>
> Which is why every later method in the lecture explains a **single decision**
> rather than the model. CAM and LRP both take one input and ask which parts of
> *it* mattered. The lecture reaches this shift without announcing it, and it is
> the most important thing on the page.

> [!note] And it echoes symbolic AI's own failure
> *"Small domains only"* ([[symbolic-ai]]) and *"difficult to show for larger
> networks"* are the same complaint: representations a person can read do not
> survive scale. The lecture states both and does not connect them.

## See also

- [[weight-based-transfer]] ? [[knowledge-extraction]] ? [[transducer-network]]
