---
title: Potential field navigation
type: concept
sources: [L08]
tags: [robotics, navigation, geometry]
updated: 2026-09-21
---

# Potential field navigation

The geometric reading of [[motor-schema]]s.

> **Output of schemas can be visualised as vector / potential field.**
> **Two kinds of fields: attractive and repulsive.**
>
> At each point: **`U_total = U_attraction + U_repulsion`** ? **movement
> predetermined**.

Each schema paints the whole workspace with a vector at every point. Goals
produce inward-pointing fields; obstacles produce outward-pointing ones. Sum
them and every position in the world already has an answer to *what do I do
here?* ? hence **predetermined**: the robot does not plan a path, it **falls
down** one.

The notes' sketches show exactly this: arrows converging on *Goal*, arrows
radiating from an obstacle, uniform arrows for Move-Ahead, random arrows for
Noise, and two opposed arrow-fields on either side of a corridor for
Stay-On-Path.

## Pseudocode

```
def field(p):                                  # p = robot position
    v = Vector(0, 0)
    for schema, gain in zip(schemas, G):
        v += gain * schema.vector_at(p)        # each schema knows the whole plane
    return v

while True:
    actuate(field(current_position()))         # no path, no plan, no search
```

There is no search anywhere in this loop. That is the attraction of the method
and the source of its failure mode.

## What "predetermined" costs

A potential field is a **greedy descent**. It has no lookahead, so it cannot
know that the way to the goal is briefly away from it. Formally: gradient
descent on `U_total` finds a **local** minimum, and nothing guarantees that the
local minimum is the goal. See [[local-minima-problem]].

> [!warning] The notes mix a potential and a field
> `U` conventionally denotes a scalar **potential**, whose negative gradient is
> the force. The lecture writes `U_total = U_attraction + U_repulsion` and then
> treats the result as a **vector** to be executed. Both readings work ? summing
> potentials and then differentiating is equivalent to summing the gradients,
> because differentiation is linear ? but only because the combination is linear.
> The notes do not say which object they mean, and for Avoid-Obstacle's `?` case
> the potential does not exist.

> [!note] The one schema that is not a field
> **Noise** has no potential: it is resampled each cycle rather than being a
> function of position. That is exactly why it can escape a minimum ? and why it
> is a patch rather than a solution.

## Elsewhere in the module

The same picture ? an energy surface, movement downhill, minima that trap ?
is [[backpropagation|gradient descent]] in L03, where the surface is over **weights** rather
than **space**, and the trap is a bad model rather than a stuck robot.

| | L03 | L08 |
|---|---|---|
| Surface over | weights | physical position |
| Descent by | [[backpropagation]] | driving |
| Local minimum | a poor model | a stalled robot |
| Escape by | momentum, restarts, noise | **noise schema** |

The remedies are the same remedy. The module does not remark on it.

## See also

- [[motor-schema]] ? [[local-minima-problem]] ? [[behaviour-coordination]]
