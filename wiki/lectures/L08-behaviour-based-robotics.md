---
title: "L08 ? Behaviour-based Robotics"
type: lecture
lecture: L08
sources: [L08]
tags: [robotics, agents, embodiment, architecture, navigation]
updated: 2026-09-21
---

# L08 ? Behaviour-based Robotics

Source: `raw/lectures/BioinspiredAIWdh8.pdf`, 10 pages, all marked `L8`.

The first lecture in the module about **whole agents acting in the world**
rather than about a perceptual or learning mechanism. Its argument is a negative
one: the obvious way to build a robot ? decompose the problem into perception,
modelling, planning, execution ? is the wrong way, and a pile of simple,
tightly-coupled sensor-to-motor behaviours does better.

## The argument

### 1. The classical robot and why it fails

> **Classic Robot: Move ? Think ? Next Move**

[[functional-decomposition]] splits control top-down into logical units:

| # | Unit | Stage |
|---|---|---|
| 1 | Processing sensory data | Perception |
| 2 | Creation of world model | Modelling |
| 3 | Planning | Planning |
| 4 | Plan execution | Execution |

Sensors ? Robot Controller ? Actuators, with everything happening inside the
controller.

**(+)** precise, controllable, predictable.
**(?)** difficult to handle noise and uncertainty; **unit failure ? system
failure**; computationally expensive; **granularity problem (model)** ? how
fine-grained should the world model be?

The serial chain is the root of all four faults: every stage waits on the one
before, and every stage is a single point of failure.

### 2. [[braitenberg-vehicle]]s ? the existence proof

Two sensors, two motors, four wires. Straight wiring gives **fear** (drive fast
away from light, slowing with distance); crossed wiring gives **aggression**
(drive fast at the light, hitting it at full speed). Make the connections
**inhibitory** ? *stronger stimulus ? smaller input to actuator* ? and the same
two wirings become **admires** (slows as it approaches until it stops) and
**explores** (slows near a source but turns away, looking out for a stronger one).

> Behaviour **is goal-directed, fast, flexible and adaptive**, and **might even
> appear intelligent**. **No cognitive processes. Agent is purely reactive.
> Simple architecture leads to robustness.**

This is the module's cleanest demonstration of [[intelligent-behaviour]] without
intelligence, and the basis for everything after it. See [[reactive-agent]].

### 3. Brooks's assumptions

> - Complex behaviour does **not** require a complex control system
> - Simple ? increase stability and robustness
> - Robots should be autonomous to survive long without human
> - Environment is 3D
> - Absolute coordinate system leads to cumulative errors

The last is the sharpest technical point: dead reckoning in a global frame
integrates error without bound, so use the world itself as its own model.

### 4. The formalism

- Stimuli relevant to behaviours `S = [s_1, ?, s_n]`
- Set of behaviours `B = [b_1, ?, b_n]`
- Weights / gains of primitive behaviours `G = [g_1, ?, g_n]`
- Response of behaviour `i`: `r_i = b_i(s_i)`
- Overall response: **`? = C(G * B(S))`**, where `C` **selects / combines
  outputs to produce a single response vector**

Each `r_i` is *(magnitude, angle)* ? a vector, not a scalar.

Everything else in the lecture is a choice of `C`. See
[[behaviour-coordination]].

### 5. Competitive `C` ? pick one

| Scheme | Rule |
|---|---|
| **Subsumption** | higher-level behaviours can **overrule** lower-level output |
| **Action selection** | selection through a criterion (e.g. signal strength) ? `MAX(B1,B2,B3)` |
| **Voting** | behaviours **vote** for action response `R`; take `Max(R1,R2,R3)` |

[[subsumption-architecture]] is the worked case.

### 6. Cooperative `C` ? blend them

[[motor-schema]]s output vectors and sum them:

> **`R = ? (G_i ? R_i)`** ? each response `R_i` weighted by gain factor `G_i`

Visualised as a [[potential-field-navigation|potential field]] of attractive and
repulsive components, `U_total = U_attraction + U_repulsion`, from which
**movement is predetermined**.

### 7. What goes wrong

**Local minima** (robot stalls between attraction and repulsion) ? *possible
solution:* a **noise schema**. **Cyclic behaviour** ? *possible solution:* an
**avoid-past schema**. See [[local-minima-problem]].

### 8. From reactive to learned ? NICO

The back half of the lecture jumps to [[nico]], a child-sized humanoid research
platform, and to learning object picking: the
[[object-picking-architecture|modular ? end-to-end spectrum]],
[[neural-grasp-learning|a self-supervised grasp-learning cycle]],
[[task-inference-network|unsupervised task inference]], and
[[imitation-learning]].

The connection to behaviour-based robotics is never stated. See below.

## Pseudocode ? the whole lecture in one loop

```
# Behaviour-based control (the general form, ? = C(G * B(S)))
while True:
    S = read_sensors()
    R = [b_i(S) for b_i in behaviours]      # each returns (magnitude, angle)
    response = C(G, R)                       # the design decision
    actuate(response)
```

```
# Competitive C
def C_subsumption(G, R, priority):           # priority: high -> low
    for i in priority:
        if active(R[i]): return R[i]         # higher layer suppresses the rest
    return REST

def C_action_selection(G, R):
    return R[argmax(magnitude(r) for r in R)]

def C_voting(G, R):
    votes = tally(R)                         # each behaviour votes over actions
    return argmax(votes)
```

```
# Cooperative C
def C_motor_schema(G, R):
    return sum(G[i] * R[i] for i in range(len(R)))   # vector sum
```

> [!note] The two families are the same fork the module keeps hitting
> Competitive `C` = pick the winner. Cooperative `C` = weighted sum.
> That is **exactly** L07's [[fusion-strategies]] ? max-fusion versus
> sum-fusion ? arrived at independently, for motor output instead of sensory
> input. Neither lecture mentions the other.

## Behaviour-based agents ? the balance sheet

**Advantages**

| | |
|---|---|
| **Fast reaction** | immediate mapping of sensory info onto motor actions |
| **Robustness** | if one part fails, robot may retain some behaviour (*competence*) |
| **Multiple goals** | can follow multiple goals simultaneously (*coordination / schemas*) |
| **Extensibility** | easy to add new parts on top |
| **Simplicity** | complexity derives from continuous interaction of simple modules with env and each other |
| **Computational tractability** | usually simple calculations, parallelisable |

**Disadvantages**

| | |
|---|---|
| **Limited information** | agents without env models must have sufficient info from local env |
| **Non-local information** | how does the agent take into account non-local info? |
| **Learning globally** | difficult to make a reactive agent that learns globally |
| **Complex dynamics** | hard to engineer agents with large numbers of behaviours |

Read the two lists together and they are the same fact twice: **there is no
world model.** That buys speed, robustness and parallelism, and costs
everything that requires knowing what is not currently visible.

> [!note] "Learning globally" is the module's oldest open problem, again
> A reactive agent that cannot learn globally is the motor-side twin of the
> local-vs-global learning thread running since L02. The wiring of a
> [[braitenberg-vehicle]] is set by hand; nothing in the lecture learns it.

## Errors and problems in the source

> [!warning] `?` is not an implementable magnitude
> Avoid-Obstacle sets `V_magnitude = ?` for `d ? R`. In a **linear
> combination** (`R = ? G_i R_i`) an infinite term destroys every other
> contribution and is numerically undefined. It also makes the function
> **discontinuous**: the middle case gives exactly `G` at `d = R`, then jumps to
> `?`. Real implementations clamp it.

> [!warning] `S`, `B` and `G` cannot all have length `n`
> The notes write `S = [s_1?s_n]`, `B = [b_1?b_n]`, `G = [g_1?g_n]`, implying
> one stimulus per behaviour. Behaviours generally read overlapping subsets of
> sensors, and the count of stimuli has no reason to equal the count of
> behaviours. `r_i = b_i(s_i)` hard-codes the one-to-one assumption.

> [!warning] `*` in `? = C(G * B(S))` is never defined
> From `R = ?(G_i ? R_i)` on p5 it must be elementwise scalar-by-vector
> multiplication, but the lecture writes it as if it were a product of two
> vectors.

> [!warning] The imitation-learning pipeline **is** functional decomposition
> p9 gives: *Human Demo ? Symbolic Reasoning ? Action Plan ? Inverse Kinematics
> ? Robot Imitation.* That is perception ? modelling ? planning ? execution ?
> the pipeline p1 rejects as brittle, expensive and subject to the granularity
> problem. The lecture presents both, nine pages apart, **without comment**.
> The same tension recurs on p8's *modular vs end-to-end* spectrum, which is the
> same axis under a new name.

> [!warning] "Noise schema" and "avoid-past schema" are patches, not solutions
> The notes label them *possible solutions* to local minima and cycling. Neither
> has a guarantee: noise perturbs the robot out of a minimum *sometimes*, and an
> avoid-past term is extra state in an architecture whose premise is not keeping
> state. The [[local-minima-problem]] is structural, not incidental.

> [!warning] The self-learning cycle's data distribution is self-limited
> [[neural-grasp-learning]] labels grasps by having NICO **place** an object
> itself, then regrasp it. The training set therefore contains only
> configurations the robot could already reach and release ? precisely the ones
> it least needs to learn. Not raised in the source.

## Unclear in the source

- **"Environment is 3D"** is listed as a Brooks assumption with no explanation
  of what follows from it.
- **"Uncanny valley"** is named as something NICO's child-like appearance avoids,
  and never defined. See [[uncanny-valley]].
- **Voting**: how behaviours vote, and over what action set, is not given. The
  diagram shows all-to-all arrows into `Max(R1,R2,R3)`, which looks identical to
  action selection.
- **`C` for subsumption** is drawn with `S` nodes (suppression) but the
  suppression *timing* ? how long a higher layer holds down a lower one ? is
  never mentioned, and it is the whole difficulty of the real architecture.
- **"Self-Organized Network of Behaviors"** appears in the task-inference
  overview with no algorithm. Whether this is a [[self-organising-map]] is not
  stated. See [[task-inference-network]].
- **Gains `G`** are never learned, tuned or discussed. They are the only free
  parameters in the whole formalism.
- **No dates, no citations.** Brooks and Braitenberg are named; nothing else is
  attributed, and no paper or year appears.

## Verified from the source

- **Guarded Towards-Goal is continuous.** `V = G` for `d ? S` and `G?d/S` for
  `d < S`; at `d = S` both give `G`. ? And it decays linearly to `0` at the
  goal, which is what "guarded" should mean.
- **Avoid-Obstacle's middle case is continuous with the outer case.**
  `(S?d)/(S?R) ? G` at `d = S` gives `0`, matching the `d > S` case. ?
  (It is only the `d ? R` case that breaks ? see the warning above.)
- **Fear and aggression really are the straight/crossed wiring.** Straight
  (ipsilateral, excitatory): the sensor nearer the light drives the motor on the
  same side ? turns away. Crossed (contralateral): drives the far motor ? turns
  towards. ? The diagrams' *slow/fast* labels are consistent with this.

## Cross-references

- [[intelligent-behaviour]] ? L01 asked what counts; Braitenberg is the answer's
  hardest test case
- [[embodiment-and-situatedness]] ? Brooks's two core ideas, and their relation
  to L04's embodied-language thesis
- [[fusion-strategies]] ? L07's sensory version of the same combine/arbitrate fork
- [[hybrid-architecture]] ? L05's learned/hand-written axis, now a third axis
- [[data-augmentation]] ? reused here for grasping
- [[learning-paradigms]] ? still no reinforcement learning, in the one lecture
  where an agent acts in a world and could obviously use it
