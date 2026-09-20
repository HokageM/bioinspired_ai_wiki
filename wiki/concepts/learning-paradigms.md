---
title: Learning Paradigms
type: concept
tags: [learning, foundations]
sources: [L03, L04, L06, L08, L09, L10, L11, L12, L13]
created: 2026-09-20
updated: 2026-09-21
status: solid
---

# Learning Paradigms

The three kinds of feedback a learner can receive (L03, p6). The distinguishing
variable is **how much information the feedback carries, and when it arrives**.

## Biological origin

Each has a rough biological counterpart, though the notes only make the third
explicit (via *reward ↔ dopamine* in [[ann-brain-correspondence]]).

## The three (L03, p6)

### Supervised
> **Detailed feedback that includes the correct response to each input;
> omnipresent teacher.** *eg: "turn left"*

The teacher supplies the answer, for every input, immediately. This is the
setting of [[perceptron-learning-rule]], [[backpropagation]] and
[[loss-function]].

### Unsupervised
> **No feedback; the ANN tries to intelligently cluster inputs and learn proper
> correlations between components of input space.** *eg (margin): "after a long
> corridor, a left"*

No teacher at all. The structure has to come from the data. This is the setting
of [[hebbian-learning]] and [[stdp]] from L02 — both learn from *correlation*,
exactly as this definition says — and of the [[autoencoder]], which manufactures
its own target from the input.

### Reinforced
> **Simple feedback mainly at the end of a problem-solving attempt.**
> *eg: "you are at the goal"*

Feedback exists but is sparse, scalar and delayed.

## The trade-off

| Paradigm | Feedback content | Timing | Module examples |
|---|---|---|---|
| Supervised | Full target vector | Every input | [[backpropagation]], [[perceptron-learning-rule]] |
| Unsupervised | None | — | [[hebbian-learning]], [[stdp]], [[autoencoder]] |
| Reinforced | One scalar | End of attempt | *none yet in the module* |

Reading down the "feedback content" column gives the central tension: rich
feedback makes learning easy but is rarely available; the unsupervised case is
ubiquitous but has to invent its own objective.

## Computational form

```text
# SUPERVISED — target given per input
for (x, t) in D:
    y = forward(x)
    update(gradient_of(loss(y, t)))         # t supplied by a teacher

# UNSUPERVISED — no t exists; the objective comes from the data itself
for x in D:
    y = forward(x)
    update(hebb(x, y))                      # correlation      (L02)
    # or: update(gradient_of(loss(decode(encode(x)), x)))
    #                                        ^ target IS the input ([[autoencoder]])

# REINFORCED — one scalar, at the end
trajectory = []
while not done:
    a = policy(state)
    state, done = step(a)
    trajectory.append((state, a))
r = reward()                                 # e.g. "you are at the goal"
update_all(trajectory, r)                    # credit assignment: which action
                                             # in the trajectory earned r?
```

The last comment is the hard part of reinforcement learning and the reason it is
harder than the other two — the module does not develop it.

## Where it appears in the module

- [[L03-computational-neural-networks]] — page 6.

## See also

- [[hebbian-learning]], [[stdp]] — L02's unsupervised rules.
- [[backpropagation]], [[perceptron-learning-rule]] — L03's supervised rules.
- [[autoencoder]] — unsupervised via a self-supplied target.
- [[ann-brain-correspondence]] — *reward ↔ dopamine*.
- [[self-supervised-learning]] (L04) — the fourth paradigm.
- [[self-organising-map]] (L04) — the module's first actual unsupervised
  *algorithm*.
- [[transfer-learning]] (L04) — cuts across this taxonomy.

## Additions from L04

L04 supplies what this page was missing:

| Gap left by L03 | Filled by L04 |
|---|---|
| Unsupervised learning defined but no algorithm | [[self-organising-map]] — a full learning procedure |
| Self-supervised not distinguished | [[self-supervised-learning]] — now its own page, via [[gpt]] and [[hubert]] |
| Reinforcement learning named only | **still unfilled** |

The taxonomy should now be read as four boxes, not three:

| Paradigm | Teacher signal | L04 example |
|---|---|---|
| Supervised | external targets | [[imitation-network]] |
| Unsupervised | none | [[self-organising-map]], [[multi-layer-associator]] |
| Self-supervised | targets built from the input | [[gpt]], [[hubert]], [[word2vec]] |
| Reinforcement | scalar reward | — |

> [!note] The source is inconsistent
> L04 calls GPT's generative pre-training **"unsupervised"** on p9 and HuBERT
> **"self-supervised"** on the same page, for structurally identical setups.

## Open questions / gaps

- **Reinforcement learning is named but never developed** — no algorithm, no
  value function, no policy. The credit-assignment problem is not mentioned.
  Whether a later lecture picks this up is an open thread in [[overview]].
- Self-supervised learning is not distinguished from unsupervised, even though
  the [[autoencoder]] on the same page is the standard example of it.
- The unsupervised margin example ("after a long corridor, a left") is
  abbreviated and its point is unclear.


## L06: the same layer, either paradigm

[[L06-hierarchical-vision]] states of the [[neocognitron]]:

> **Training of S-cells with unsupervised or supervised methods; only S-cells
> have learning inputs.**

This is a useful data point for this page. The *same* layer, computing the
*same* function on the *same* inputs, can be fitted either way. The paradigm is
therefore a property of the **training signal**, not of the architecture ? which
is exactly what the L04 comparison of [[gpt]] ("unsupervised") and [[hubert]]
("self-supervised") already suggested, since those two have near-identical
setups and differ mainly in what they are called.

The second clause is the sharper one: **only S-cells learn at all.** The C-cells
that provide invariance are hard-wired. So the Neocognitron splits into a
learned part and a designed part, and the module's modern counterpart
([[pooling]]) makes the same split ? pooling layers have no parameters either.

Not every useful function has to be learned, and the module's architectures
quietly assume this without ever saying so.


## L08: the absence becomes conspicuous

[[L08-behaviour-based-robotics]] is the first lecture with an **agent, actions,
a world and outcomes**. It is the natural home of reinforcement learning, which
this page has had defined and unused since L03.

It does not appear. What appears instead:

| System | Paradigm |
|---|---|
| [[braitenberg-vehicle]], [[motor-schema]], [[subsumption-architecture]] | **none** ? hand-wired, hand-tuned gains |
| [[neural-grasp-learning]] | **self-supervised** ? the robot generates its own labels by acting |
| [[task-inference-network]] | **unsupervised** ? behaviours self-organise from demonstrations |
| [[imitation-learning]] | **imitation** ? neither supervised by a teacher's labels nor driven by reward |

Two of these do not fit the L03 taxonomy at all. **Imitation learning** has a
demonstration rather than a target; **continual learning** ? named for the first
time in L08 ? is a constraint on the training regime rather than a source of
signal.

> [!note] The reactive-agent disadvantage list states the RL-shaped hole outright
> *"Learning globally: difficult to make a reactive agent that learns globally."*
> That is the credit-assignment-over-time problem, which is what reinforcement
> learning is for. The module identifies the problem and does not name the
> field that addresses it. See [[reactive-agent]].


## L09 adds nothing, and that is worth recording

Attention is presented entirely as **architecture**: saliency maps,
competition, top-down bias. Nothing in L09 is learned — the saliency model's
feature weights, its `combine` step, and the top-down bias signal are all given,
none is fitted.

So the tally at L09: of the three paradigms defined in L03, supervised appears
constantly, unsupervised occasionally, and **reinforcement learning has now been
absent for six consecutive lectures** — including L08 (an agent acting in a
world) and L09 (allocating a limited resource to maximise task performance,
which is a resource-allocation problem with a reward).


## L10 ? the striatum appears; reinforcement learning still does not

[[L10-gesture-recognition]]'s final summary contains the module's first mention
of a basal-ganglia structure:

> **Reservoir computing models by neurophysiological principles for action
> selection in prefrontal cortex and striatum.**

> [!warning] The hole is now conspicuous
> The striatum is the canonical substrate of reward-based learning, and
> **action selection** is the function reinforcement learning exists to perform.
> The module has now arrived at that problem from four directions ?
>
> | Lecture | Route to action selection | Solution offered |
> |---|---|---|
> | L03 | RL defined in the taxonomy | none; never used again |
> | L08 | [[behaviour-coordination]] | hand-set gains, fixed priorities |
> | L09 | [[winner-take-all]] | argmax over salience |
> | L10 | striatum, prefrontal cortex | [[reservoir-computing]], undefined |
>
> ? and has not once written down a reward, a value function or a policy.
> Reinforcement learning has been named-only since L03 and remains so at L10.

### Unsupervised learning gains ground

Against that, L10 substantially strengthens the **unsupervised** column.
[[gwr-network|GWR]] and [[gamma-gwr|Gamma-GWR]] learn structure, sequence *and
their own size* without labels, and
[[contrastive-language-image-pretraining|CLIP]] learns a shared vision-language
space from paired data with no manual annotation at all ? the caption is the
label. That is closer to [[self-supervised-learning]] than to either classical
paradigm, and the module does not classify it.


## L11 ? a fourth kind of optimisation, and still no reinforcement learning

[[evolutionary-algorithm|Evolutionary algorithms]] do not fit the supervised /
unsupervised / reinforcement taxonomy at all, and the lecture does not try to
place them.

| | Signal required | Optimises |
|---|---|---|
| Supervised | a target per example | weights, by gradient |
| Unsupervised | none | structure, by competition |
| Reinforcement | a reward | a policy ? *still never implemented* |
| **Evolutionary** | **one scalar per candidate** | **anything encodable** |

An EA needs neither labels nor derivatives ? only that solutions can be
**ordered**. That is why it can search a network **topology**, a discrete object
no gradient reaches, and it is the module's first genuinely gradient-free method.

> [!warning] The hole is now at its widest
> L11 has the **exploration?exploitation trade-off**
> ([[selection-pressure]]) ? the axis reinforcement learning is organised
> around ? a **scalar quality signal** rather than labels, and an application to
> **robot control** ([[collision-free-navigation]]) where a policy is learned from
> a performance measure.
>
> Every ingredient of RL is now present, distributed across L03, L08, L09, L10
> and L11, and the algorithm has still never been written down. Eleven lectures,
> named-only throughout.

> [!note] EA and RL solve the same problem differently, and the contrast is the lesson
> Both optimise behaviour from a scalar signal. RL assigns credit **within** an
> episode using the structure of time; an EA assigns credit only to the **whole
> individual** at the end. That is why an EA needs no model of the task and why it
> needs so many evaluations. Stating that contrast would have cost one slide.

## L12 ? reinforcement learning, appearance thirteen

> **Explaining the internals of RL: visualization of critic maps during RL;
> observe stability of maps over time.**

RL is mentioned for the thirteenth time since L03 and still has no algorithm, no
value function, no update rule. L12 goes further than previous mentions by using
a **term internal to** RL ? *critic*, one half of an actor?critic architecture ?
without defining it, so a reader who has followed the module in order cannot
parse the sentence.

See [[class-activation-map]] for the method being applied.

## L13 ? reinforcement learning, appearance fourteen and last

> [!warning] The module ends without ever defining reinforcement learning
> L13's [[intrinsic-motivation|curiosity]] diagram draws a complete actor–critic
> agent: environment, external reward, action selection, internal reward,
> intrinsic motivation. Fourteen mentions across eleven lectures, and there has
> never been a value function, a policy, an update rule or a named algorithm.
>
> The module has taught RL's **architecture** (this diagram), its **vocabulary**
> ("critic maps", L12), its **signal type** ([[fitness-function|scalar quality
> instead of labels]], L11) and its **central trade-off**
> ([[selection-pressure|exploration/exploitation]], L11) — everything except the
> thing itself. The final page spends the last opportunity on a *variant*.
>
> This is the largest gap in the wiki and it is now permanent for this source set.

## L13 — and a fourth regime the module never names

[[associative-gwr|Associative GWR]] learns its representation **without labels**
and then attaches a label histogram to each unit. Labels never influence the
weights. That is neither supervised nor unsupervised learning but a **layering**
of the two, and the same move appears in [[self-organising-map|SOM labelling]],
in [[word2vec|pretraining then fine-tuning]] ([[bert]]), and in
[[knowledge-extraction]] (L12).

*Learn the structure unsupervised, read it out supervised* is arguably the
module's most-used recipe, and it has no name in any lecture.
