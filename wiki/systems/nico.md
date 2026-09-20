---
title: "NICO ? Neuro-Inspired Companion"
type: system
sources: [L08]
tags: [robotics, embodiment, multimodal, agents]
updated: 2026-09-21
---

# NICO ? Neuro-Inspired Companion

A **human-like social robot** used as a **multimodal research platform**.

## Design goals

> Anthropomorphism ? scaled to realistic environment ? human-like response ?
> bimanual manipulation ? locomotion ? **modular open-source design** ?
> adaptable for individual experimental set-ups ? **affordable**

The last two are research-infrastructure goals rather than scientific ones, and
they explain the platform's existence: a shared, cheap, modifiable body that many
experiments can run on.

## Hands

> - **Artificial fingertips** with **deformation and vibration sensing**
> - **Haptic sensors** for grasping and recognising objects

Note *recognising*, not merely grasping: touch is treated as a **perceptual**
modality, not only a control signal. This is a modality the module has not
touched before ? L05 was audition, L06 vision, L07 combined the two.

> [!note] A fourth modality arrives with no theory attached
> [[multisensory-integration]] (L07) gave a principled account of fusing vision
> and audition ? [[optimal-cue-integration]], the [[spatial-principle]],
> [[superior-colliculus|SC]] maps in register. Haptics appears one lecture later
> and none of that machinery is applied to it, although touch is the modality
> for which *reliability weighting* is most obviously needed (contact is
> certain; vision of an occluded object is not).

## Head

> - **Child-like appearance ? avoids [[uncanny-valley|uncanny-valley effect]]**
> - **Sensors** (2 cameras, 2 microphones)
> - **Emotion display**

Two cameras and two microphones is exactly the sensor set L05?L07 require:
binocular disparity, and the [[interaural-time-difference|ITD]]/ILD pair that
[[cross-correlation-localisation]] needs. NICO is the body those three lectures
were implicitly describing.

## Research programme

> **Bio-inspired development of visuo-motor abilities.**
> **Developmental learning from environment interaction.**
> **Goal: unified neural architecture for object picking ? fusing vision, motor
> and semantics.**

**Object picking task ? needed components:**

> 1. **Identify object**
> 2. **Locate object in space**
> 3. **Compute and execute motor commands**

Which is ? precisely ? *what*, *where*, and *act*: the
[[two-visual-streams|ventral and dorsal streams]] of L06 plus an actuator. The
lecture does not point this out, and it is the clearest instance in the module of
an engineering requirement recapitulating a neuroanatomical division.

## Pseudocode

```
# The object-picking loop the platform is built around
def pick(utterance):
    target   = identify(camera, semantics=utterance)   # ventral / "what"
    position = locate(camera_left, camera_right)       # dorsal  / "where"
    joints   = motor_command(position, target)
    execute(joints)
    return grasped(fingertip_deformation, fingertip_vibration)   # haptic check
```

The return line is the part only an embodied system gets: success is **measured
by touch**, not asserted. That is [[embodiment-and-situatedness]] doing real
work rather than being a slogan.

## See also

- [[object-picking-architecture]] ? [[neural-grasp-learning]] ?
  [[task-inference-network]] ? [[imitation-learning]]
- [[embodiment-and-situatedness]] ? the theory NICO is built to test
- [[L08-behaviour-based-robotics]]
