---
type: system
title: Self-Organising Map (SOM)
sources: [L04, L07, L08, L09, L10, L13]
tags: [unsupervised, competitive-learning, dimensionality-reduction, self-organisation]
---

# Self-Organising Map (SOM)

An unsupervised network that maps points from a **high-dimensional source space**
into a **2- or 3-dimensional target space** while preserving topology: *distance
and proximity relationships are preserved as much as possible*.

Also called a **Kohonen map**, after [[teuvo-kohonen]].

L04's framing: *SOMs resemble self-organisation in the brain.*

## Structure

- An **m-dimensional grid** of units `u`. For `m = 2` the units are `u_ij`.
- Each unit carries an **n-dimensional weight vector** `w_ij`, living in the
  *input* space.
- **Dimensional reduction**: `n >> m`.
- Training stimuli: a set of `k` n-dimensional vectors
  `x_k = {x_k,1, …, x_k,n}`.

The grid is the map; the weights are the positions of the map's nodes in the
high-dimensional space. Training drags the net of nodes over the data cloud.

## Learning algorithm (L04, p5)

1. **Initialisation** — random weight vectors.
2. Present a random sample `x` to the network.
3. Iterate through the grid, compute distance `d_ij = ‖x − w_ij^t‖`.
4. Select the **best matching unit (BMU)** `u`, such that the distance is
   minimal: `arg min_ij d_ij`.
5. Compute the **neighbourhood** `h_u` of the BMU.
6. Shift the weights of units in `h_u` towards `x`:

```
w_ij^(t+1) ← w_ij^t + h_u(t) · η(t) · (x − w_ij^t)
```

where `h_u(t)` is the **neighbourhood function** and `η(t)` the **learning rate**.

⇒ Over time the network **self-organises while preserving input topology**.

> [!note] Source reads `avg min_ij d_ij`
> Almost certainly *arg min*. Recorded as `arg min` here.

## Pseudocode

```
# ---- Kohonen SOM training ----
# X      : set of k training vectors, each in R^n
# grid   : m-dimensional lattice of units (typically m = 2)
# T      : number of training steps
# eta0   : initial learning rate
# sigma0 : initial neighbourhood radius

for each unit p in grid:
    w[p] <- random vector in R^n

for t in 0 .. T-1:

    x <- random sample from X

    # --- competition: find the best matching unit ---
    bmu <- argmin over p in grid of  norm(x - w[p])

    # --- cooperation + adaptation ---
    eta   <- eta0   * decay(t, T)          # learning rate shrinks
    sigma <- sigma0 * decay(t, T)          # neighbourhood shrinks

    for each unit p in grid:
        d_grid <- lattice_distance(p, bmu)     # distance ON the grid, not in R^n
        h      <- neighbourhood(d_grid, sigma) # 1 at bmu, falling off with d_grid
        w[p]   <- w[p] + h * eta * (x - w[p])

# ---- inference ----
def project(x):
    return argmin over p in grid of norm(x - w[p])   # grid coordinates of x
```

Two distances are in play and they must not be confused:
`norm(x − w[p])` is in **input space** and decides *who wins*;
`lattice_distance(p, bmu)` is on the **grid** and decides *who else learns*.
Topology preservation is exactly the consequence of that second one.

## Adaptation — `η(t)`

- Visualised as a **distorted Kohonen map**: distance between units ~ distance
  in the higher dimension.
- Neurons are moved closer to the input pattern in high-d space.
- Controlled by a learning parameter `η(t)` **which can decay over time**.

## Activation spreading — `h_u(t)`

- Activation of the neuron is spread into its direct **neighbourhood**.
- Neighbours become sensitive to the same input patterns — the lecture
  attributes this to **[[hebbian-learning]]**.
- The size of the neighbourhood is **initially large and reduced over time**.

This two-phase schedule (wide-and-fast → narrow-and-slow) is what makes the map
first unfold globally and then refine locally. The notes state both decays but
never say the two must be annealed *together*.

## Why it belongs in a bio-inspired module

- It is **unsupervised** — no teacher signal, unlike [[backpropagation]].
- Its update is **local**: only the winner and its lattice neighbours change.
- It produces **topographic maps**, which is what sensory cortex actually does.
  The module's own [[place-cells]] and [[grid-cells]] from L01 are biological
  topographic codes; L04 does not make the connection but it is the obvious one.

## Role in L04

The SOM is the bridge between the embodied story and a runnable model. In
[[cross-modal-stimuli-prediction]] a single SOM sits between visual and auditory
encoders/decoders, so that a concept in one modality can activate the latent
representation of another. *Neighbourhoods form ca…* (source cut off — almost
certainly **categories**).

## Open questions

- The **neighbourhood function is never given a form**. Gaussian
  `h = exp(−d²/2σ²)` is standard `[external]`.
- No convergence guarantee is stated — contrast
  [[perceptron-convergence-theorem]], which L03 did state. SOMs have no general
  convergence proof `[external]`.
- No stopping criterion.
- Grid topology (rectangular vs hexagonal) is not discussed.

## See also

[[teuvo-kohonen]] · [[cross-modal-stimuli-prediction]] ·
[[hebbian-learning]] · [[learning-paradigms]] · [[network-architectures]] ·
[[embodied-language-representation]] · [[L04-embodied-language-processing]]


## L07: a third job, and a structural upgrade

The SOM returns in [[L07-crossmodal-processing]] as the **integration layer of
the [[superior-colliculus]]** ? taking a visual estimate `x_v` and an auditory
estimate `x_a` and producing a fused `x_s`.

> **The SOM learns which representations cluster together. It could learn map
> registration.**

That is a genuinely different use. In L04 the SOM organised one modality's
feature space; here it is asked to **align two spaces** so that the same
position means the same unit in both. Map registration is the hard part of
multisensory integration and the notes raise it in one clause and drop it.

More importantly, L07 changes the algorithm. In the
[[histogram-based-som]], each unit stores **a histogram per input dimension**
instead of a single prototype vector, is trained by **updating histograms rather
than single-valued prototypes**, and outputs the **likelihood of the input given
the histograms** rather than a distance.

| | SOM (L04) | Histogram-based SOM (L07) |
|---|---|---|
| Unit holds | prototype vector | distribution |
| Output | distance / BMU | likelihood ? population-coded PDF |
| Can express uncertainty | **no** | **yes** |

The upgrade matters because it lets the same local, competitive, biologically
comfortable learning rule carry the variance information that
[[optimal-cue-integration]] needs. A standard SOM knows *where* a unit sits; a
histogram SOM knows *how sure* it is.


## A fourth job, probably (L08)

[[task-inference-network]] in [[L08-behaviour-based-robotics]] is built around a
**"Self-Organised Network of Behaviours"** ? demonstrations are organised into a
graph of linked nodes, from which the current task is inferred and used to
condition a policy.

The lecture gives no algorithm and never says "SOM", so the wiki does not assert
that it is one. If it is, the jobs this mechanism has been given are:

| Lecture | Organises | Output |
|---|---|---|
| L04 | a feature space | winning unit |
| L04 | two modalities | [[cross-modal-stimuli-prediction|a mapping]] |
| L07 | sensory evidence | [[histogram-based-som|a likelihood]] |
| **L08** | **behaviours** | **a task code** |

The novelty of the last is that the space being organised is one of **actions**,
not percepts ? and that its coordinates are then used as a *conditioning vector*
rather than a classification. That is what makes a growing task set possible: a
new task is a new region, and a nearby task gets a nearby vector.


## Winner-take-all, finally named (L09)

The SOM's best-matching-unit step is [[winner-take-all]], and
[[L09-bio-inspired-attention]] is the first lecture to name that operation as an
algorithm in its own right — *"only the most salient or active one is
selected"*, called **the core algorithm** of a saliency model.

It also supplies the reason the primitive keeps reappearing: WTA is
implementable by **local lateral inhibition**, needing no comparator and no
global read-out. That is precisely why competitive learning is biologically
comfortable, and this page has assumed it since L04 without the justification.


## L10 ? the SOM grows

[[L10-gesture-recognition]] introduces [[gwr-network|Growing When Required]] as a
direct modification:

> **SOM size is driven by the input and not fixed. Growth and shrinking: adding
> or removing nodes.**

This is the first change to the **architecture** rather than to the weights in
the entire module. Every learning rule so far ? [[hebbian-learning]],
[[perceptron-learning-rule]], [[backpropagation]], and the SOM's own update ?
adjusts the strength of connections laid down by a human choosing a layer width
or a grid size. GWR chooses the size itself.

| | SOM (L04) | GWR (L10) | Gamma-GWR (L10) |
|---|---|---|---|
| Units | fixed grid | grows / shrinks | grows / shrinks |
| Topology | fixed lattice | Hebbian edges, aged | Hebbian edges, aged |
| Input | static vector | static vector | **vector + context** |
| Sequences | no | no | **yes** |

[[gamma-gwr|Gamma-GWR]] adds a **context vector** to each node and includes it in
the distance, so the winner depends on history. Competitive learning gains a
memory without gaining a gradient.

### The SOM's sixth and seventh jobs

1. **L04** ? dimensionality reduction and topographic mapping.
2. **L04** ? [[histogram-based-som|histogram-based]] classification.
3. **L05** ? a model of [[tonotopic-representation|tonotopic]] organisation.
4. **L07** ? a component in a [[hybrid-architecture|hybrid]] crossmodal system.
5. **L08** ? proposed for a self-organised network of behaviours.
6. **L10** ? **an incrementally growing memory** (GWR).
7. **L10** ? **a sequence learner** (Gamma-GWR).

> [!success] L08's unnamed architecture, probably named
> ~~[[task-inference-network]] (L08) refers to "a self-organised network of
> behaviours" supporting continual learning, without naming an algorithm.~~
> GWR is from the same research programme, does exactly that, and is motivated by
> exactly that problem. Recorded as a strong inference; neither lecture states
> the connection.

## L13 — what the SOM was for, revealed at the end

> **GWR: dynamic variant of SOM.** Graph structure of neurons that learns to
> represent a dataset. Start with few neurons; "grow" by creating new neurons to
> represent novel input.

L13 makes explicit what the module implied for nine lectures: the SOM is taught
because it is the **base case of a continual learner**. It has no output layer to
resize, no labels to enumerate, and it updates per sample — three of
[[continual-learning]]'s four requirements satisfied by construction.

Its one failure is the fourth: a **fixed grid** has fixed capacity, so a
sufficiently novel input has nowhere to go and must displace something. That is
exactly the gap [[gwr-network|GWR]] fills, and the difference between them is
precisely the [[continual-learning-strategies|dynamic-architecture]] strategy.

Also unremarked: the standard way to classify with a trained SOM — label each
unit by the classes it won — is [[associative-gwr]]'s mechanism exactly.
