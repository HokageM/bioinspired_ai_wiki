---
type: system
title: Hybrid Acoustic Tracking
sources: [L05]
tags: [audition, robotics, hybrid, tracking]
---

# Hybrid Acoustic Tracking

L05's full robot system: locate a sound source algorithmically, predict where it
is going neurally, and turn the head.

**Inspired by the mammalian system (2-ear model, ITD).**

- **Cross-correlation** algorithm for sound localisation
- **Recurrent NN** for tracking the sound source, to improve the localisation
  task
- **Learning and adaptation to acceleration and deceleration**

## The three stages

```
   MIC ──┐                                    digital representation
         ├→ ┌───────────────┐                 of detected signals
   MIC ──┘  │ Correlation   │
            │    (ITD)      │
            └───────┬───────┘
                    │  σ = # of offsets from cc
            ┌───────▼───────┐
  Stage 1   │ Localization  │   →  angle position representing location of source
            └───────┬───────┘
                    │  Θ = angle of incidence
            ┌───────▼───────┐
  Stage 2   │   Tracking    │   →  next predicted position / angle of source
            │     (SRN)     │      t_n+1
            └───────┬───────┘
                    │  ΔΘ = next position
            ┌───────▼───────┐
  Stage 3   │ Motor Control │
            └───────────────┘
```

| Stage | Component | Learned? |
|---|---|---|
| 1 | [[cross-correlation-localisation]] + [[geometric-sound-localisation]] | **no** |
| 2 | [[simple-recurrent-network]] | **yes** |
| 3 | motor control | not described |

## Stage 2 — the SRN tracker

**Simple RNN for prediction of sound angle for a moving sound source.** The SRN
acts as predictor for the next angle in the trajectory of the source.

- **Input:** current sound angle. **Output:** predicted sound angle.
- Layer sizes on the arrows: `45:30` input→hidden, `30:45` hidden→output,
  `30:30` hidden↔context at **1:1** (the context layer is a verbatim copy of
  the hidden layer — see [[simple-recurrent-network]]).
- Timing: `t_n` → prediction → second input `t_n+1` → *now* can predict `t_n+2`.
- ⇒ **two inputs for context needed.**

That last point is the practical cost of recurrence: the context layer is
meaningless until it has been filled, so the tracker is blind for its first
step and only useful from the third sample onwards.

## Pseudocode

```
# ---- full hybrid tracking loop ----
# c : speed of sound,  r : sampling rate,  b : microphone spacing

srn.reset_context()
history <- 0

loop forever:

    # --- Stage 1: localise (algorithmic, no training) ---
    g, h  <- record_window(MIC_left), record_window(MIC_right)
    s, d  <- cross_correlate(g, h)          # d is the ITD in samples
    a     <- d * c / r
    theta <- arccos(a / b)                  # angle of incidence

    # --- Stage 2: track (neural, learned) ---
    theta_next <- srn.step(theta)           # context carries the trajectory
    history    <- history + 1

    if history < 2:
        continue                            # context not yet primed

    # --- Stage 3: act ---
    delta <- theta_next - current_head_angle
    motor_control(delta)                    # turn towards where it WILL be

    # --- online adaptation to acceleration/deceleration ---
    if previous_prediction exists:
        srn.train_step(target = theta, prediction = previous_prediction)
    previous_prediction <- theta_next
```

The system turns towards the **predicted** angle, not the measured one. That is
the whole point of stage 2: by the time stage 1 has finished correlating and the
motors have moved, a moving source is no longer where it was.

## Why hybrid?

The lecture argues the split explicitly (p4):

- **Algorithmic sound source localisation is well understood algorithmically.**
- **Cross-correlation does not require training to provide azimuth angle.**
- **Neural predicting of source enables a quicker response and can learn
  temporal sequences.**

Trajectory prediction has no closed form — it depends on how *this* source
happens to move, and on acceleration and deceleration the designer cannot
anticipate. That is the part worth learning. See [[hybrid-architecture]].

## The demo scenario (p2 margin)

Two sketches accompany the diagram without explanatory text:

- `Task:` **1** initial direction → **2** sound detected → **3** new direction.
- `speech recognition → m = "Hello" → parse → response`

Together they suggest the intended application: a robot that turns towards a
speaker and then processes what was said — which matches the lecture's closing
claim about *improving speech recognition by user localisation and vision*.

## Relation to the fully bio-inspired version

L05 also presents [[hybrid-spiking-localisation-network]], which replaces
stage 1's algorithm with an MSO/LSO spiking network. The two systems are the
lecture's two answers, and its summary lists them as such:

- **ITD in a full hybrid acoustic system** ← this page
- **ITD & ILD in a hybrid spiking NN** ← the other

Note that the spiking version gets **both** cues while this one uses ITD only.

## Unclear in the source

- **Layer sizes are ambiguous** — see [[simple-recurrent-network]]. Why an angle
  needs 45 input units is not explained; 4° bins over 180° would fit.
- **Motor control is a box with no contents.**
- **No training regime** for the SRN is given: no loss, no dataset, no statement
  of whether it trains online or offline.
- **`σ = # of offsets from cc`** is defined and then never used.
- ILD is never used in this system, despite being introduced on p1 as one of the
  two cues.

## See also

[[cross-correlation-localisation]] · [[simple-recurrent-network]] ·
[[geometric-sound-localisation]] · [[hybrid-architecture]] ·
[[hybrid-spiking-localisation-network]] · [[interaural-time-difference]] ·
[[L05-robot-sound-localisation]]
