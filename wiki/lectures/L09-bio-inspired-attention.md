---
title: "L09 ? Bio-inspired Attention"
type: lecture
lecture: L09
sources: [L09]
tags: [attention, neuroscience, vision, audition, multimodal, methods]
updated: 2026-09-21
---

# L09 ? Bio-inspired Attention

Source: `raw/lectures/BioinspiredAIWdh9.pdf`, 7 pages, all marked `L9`. Dated
**10.07.2024** on p7.

> **Motivation: the art of being wise is the ability to know what to overlook.**

(Unattributed in the notes.)

This lecture **closes two of the wiki's oldest open threads**: *attention is
used but never defined* (open since L04's [[gpt]]) and *the cocktail party
problem is never solved* (open since L05).

## The field map

p1 draws attention as the meeting point of four disciplines:

```
      Psychology (crossmodal attention)
       ?                    ?            ?  Human?robot interaction
Cognitive                Human-centred AI
neuroscience                ?              ?
       ?                    |                CS: robotics,
Neuroscience ?? Biomedical engineering ??    computational modelling
(task-based fMRI,
 resting-state fMRI)
```

This is the module's own methodology diagram, and the only place it is drawn.

## Definitions

> - **Cognitive control: multiple processes that plan and coordinate actions to
>   meet task goals**
> - **Attention: the most important subfunction of cognitive control**
> - **Selective attention: like a filter with the ability to remove irrelevant
>   information and thus optimise the current goal**

See [[attention]].

## The four kinds

| Kind | Definition | Marginal gloss |
|---|---|---|
| **Selective** | control awareness of the internal mind and the outside world; **integrate multidimensional and multimodal information** | ability to focus on specific stimuli while ignoring others *(filter)*; *concentrate*; *focus on a person in a loud environment* |
| **Sustained** | maintain a state over time to detect the incoming stimulus; **enhance relevant stimuli and inhibit irrelevant distractors** | over a longer period; focus on a single task without being easily distracted |
| **Exogenous** | **bottom-up, stimulus-driven**; instinctive and spontaneous | *external* |
| **Endogenous** | **top-down, goal-driven**; spotlight, allocate limited (resources) | *internal goals* |

The last two are a **dichotomy**, the first two a pair of different questions
(*what is selected* versus *for how long*). The lecture lists all four as if they
were one taxonomy. See [[exogenous-and-endogenous-attention]].

## The three functional networks

> - **Alerting network:** ability to be alert for upcoming stimuli ? *(frontal
>   lobe, thalamus, parietal lobe)*
> - **Orienting network:** focus on specific information among multiple sensory
>   input ? *(TPJ, SPL, FEF)*
> - **Executive control network:** monitoring and resolving conflicts between
>   different inputs ? *(ACC, dlPFC, AI)*

See [[attention-networks]].

## Measurement

> **Reaction time (RT): interval of time between the presentation of the
> stimulus and appearance of the appropriate voluntary response in the subject.**

> **Additive-factors method: procedure for analysing reaction-time data to
> determine whether two variables affect the same or different processing
> stages.**
> - **If two variables influence different stages, their effect should be
>   additive**
> - **If two variables influence the same stage, their effect should be
>   interactive**

**Attentional Network Test:**

```
Alerting effect          = RT(no cue)       ? RT(double cue)
Orienting effect         = RT(centre cue)   ? RT(spatial cue)
Executive control effect = RT(incongruent)  ? RT(congruent)
```

See [[reaction-time]], [[additive-factors-method]], [[attention-network-test]].

## Visual attention

**Pop-out:**

> **Pop-out happens when an object has more salient physical features than other
> objects in the context.**
> **Physical features: location, colour, shape, orientation, brightness, etc.**
> **Saliency could be extended to affective and social domain, like familiarity,
> threat, etc.**

**Feature integration theory (FIT):** object ? **preattentive stage** (*analyse
into features*) ? **focused attention stage** (*feature binding*) ?
**perception** (*Wahrnehmung*). Colour maps and orientation maps project onto a
**map of locations**; attention at a location produces a **temporary object
representation (Time t, Place x)**, which is matched against a **recognition
network** (*stored descriptions of objects, with names*).

**The saliency model:**

```
Input ? Feature Map ? Saliency Map ? Central Representation
        * Winner-Take-All is the core algorithm
```

with **centre?surround differences and normalise** noted alongside.

**A saliency map in primary cortex:**

> - The saliency map **awards higher responses to more salient image locations**
> - Those responses **are those of V1 cells tuned to input features** (colour, etc.)
> - **Firing rates of V1's output neurons increase monotonically with the
>   salience value of the visual input**

See [[pop-out-effect]], [[feature-integration-theory]], [[saliency-map]],
[[saliency-model]], [[winner-take-all]].

## Auditory attention ? the cocktail party effect

> **At a noisy party, a person can concentrate on the target conversation (a
> top-down process, *endo*) and easily respond to someone calling his/her name
> (a bottom-up process, *exo*).**
>
> **Auditory scene analysis (ASA) allows the auditory system to perceive and
> organise sound information from the environment.**
>
> **Auditory attention: localise sound sources and filter out irrelevant sound
> information.**

Pipeline: **Integrated sound ? Grouping ? Segregation ? Object competition ?
{Talker, Noise, Noise}**, with **Top-Down-Attention** arriving at the
Segregation stage, and:

> **Top-down attention control can modulate processing on each stage.**

See [[auditory-scene-analysis]], [[auditory-attention-model]].

## Crossmodal and social attention

> **[[ventriloquism-effect|Ventriloquism effect]]: auditory stimulus is
> perceptually shifted towards the position of the synchronous visual stimulus.
> Distance between audiovisual stimuli is crucial for sound localisation.**

> **Social attention helps humans quickly learn how to interact with others,
> learn the language, and build social relationships. Most crucial
> manifestation: ability to follow others' eye gaze.**

Studies: *Can the robot's facial expression and gaze impact on human?human?robot
collaboration?* ? **eye connection & trust**; H1 actor, H2 guide, instructor
[[icub|iCub]] (gaze shift, verbal instructions); **robot happy face ? faster
completion; initial gaze to guide ? rated more intelligent; gaze familiarity
increased performance and perception**. And a human?robot cooperation game with
[[nao|NAO]] assistants of different personalities (introverted / extroverted /
neutral) and different autonomy. See [[social-attention]],
[[human-robot-collaboration]].

## Pseudocode ? the saliency model end to end

```
def saliency(image):
    F = [feature_map(image, f) for f in (COLOUR, ORIENTATION, POSITION, ...)]
    F = [normalise(centre_surround(m)) for m in F]      # the starred note
    S = combine(F)                                      # the saliency map
    while True:
        loc = argmax(S)                                 # winner-take-all
        yield loc                                       # attend here
        S = inhibit(S, loc)                             # move on
```

```
# Auditory scene analysis, per the diagram
def asa(sound, goal):
    units   = decompose(sound)
    streams = group(units,        bias=topdown(goal))   # "modulate each stage"
    objects = segregate(streams,  bias=topdown(goal))
    return compete(objects,       bias=topdown(goal))   # winner = foreground
```

## Errors and problems in the source

> [!warning] The ASA text and the ASA diagram give opposite orders
> The diagram reads **Grouping ? Segregation ? Object competition**. The text
> directly beneath it says *"compound sound enters the bottom-up processing in
> the form of **segregated features** and then the features are **grouped into
> streams**"* ? segregation first, then grouping. One of the two is wrong and
> the lecture does not notice.

> [!warning] The three ANT scores do not point the same way
> A large **alerting** or **orienting** effect means the network is working well
> (the cue helped). A large **executive control** effect means the opposite ?
> more cost from conflict, i.e. worse control. The three are written as one
> family of difference scores with no note that the sign convention flips.

> [!warning] Additive factors is stated as a biconditional and is not one
> *"If two variables influence different stages, their effects should be
> additive"* is the sound direction. The lecture then uses it in reverse ? infer
> separate stages from observed additivity ? which is affirming the consequent.
> Two variables can act on the same stage and still produce additive RT effects.
> The method constrains models; it does not identify them.

> [!warning] "Cortical Oscillation Model" is a heading with nothing under it
> It appears between the bottom-up/top-down dichotomy and pop-out, and is never
> mentioned again. Oscillations are a major account of attentional selection;
> the notes contain only the three words.

> [!warning] Ventriloquism is attributed to attention here and to integration in L07
> L07 explained it by [[modality-appropriateness-hypothesis|modality
> appropriateness]] and [[optimal-cue-integration]] ? a *perceptual* mechanism
> that operates whether or not you are attending. L09 files it under
> *audiovisual crossmodal selective **attention***. These are different claims:
> one says the percept is fused, the other says the fusion is a consequence of
> where attention was allocated. The module now has three accounts of one
> illusion and has adjudicated none of them.

> [!warning] "Saliency could be extended to affective and social domain"
> Offered as an extension with no mechanism, no evidence and no citation.
> Familiarity and threat are not *physical features* of an image, so this is a
> significant change to the definition of saliency, not an extension of it.

## Unclear in the source

- **The opening quotation** is unattributed. [external] It is widely credited to
  William James; the notes do not say so and the wiki does not assert it.
- **Brain-area abbreviations** (TPJ, SPL, FEF, ACC, dlPFC, AI) are expanded
  nowhere in the module.
- **"Verbal network" and "dorsal network"** appear as marginal labels on the
  exogenous/endogenous diagram with no explanation.
- **Feature integration theory is unattributed.** No name, no year.
- **The saliency model's `combine` step** ? how feature maps are merged into one
  saliency map ? is not given; only *centre?surround differences and normalise*.
- **How the top-down bias enters ASA** is stated ("can modulate processing on
  each stage") but never specified.
- **The decision-making algorithms** for robot personality are listed as
  *personality-based* and *state-based* with no further detail.

## Threads this lecture closes

> [!success] Attention is finally defined
> ~~Attention is used but never defined ? [[gpt]] is *attention instead of
> recurrence*, and that is the whole explanation.~~
> L09 gives the psychological construct in full: selection, the four kinds, the
> three networks, the measurement method, and a computational model.
>
> **But it never connects the two senses.** Transformer self-attention is not
> mentioned anywhere in L09, and [[gpt]]'s attention is not saliency, not a
> spotlight, and not limited-capacity ? it is a similarity-weighted average over
> all positions. The module uses one word for two things across two lectures and
> never places them side by side. See [[attention]].

> [!success] The cocktail party problem gets an answer
> ~~L05 posed the cocktail party problem and left it unsolved.~~
> [[auditory-scene-analysis]] and [[auditory-attention-model]] supply a
> pipeline ? group, segregate, compete, with top-down bias throughout.
> It is an architecture rather than an algorithm: no rule for grouping, no
> competition dynamics, no evaluation.

## Cross-references

- [[attention]] ? [[saliency-map]] ? [[winner-take-all]]
- [[top-down-modulation]] ? third instance, now over every stage of a pipeline
- [[orientation-tuning]] and [[the-retina]] ? L06 supplies the V1 tuning and the
  centre?surround the saliency model runs on
- [[ventriloquism-effect]] ? the same illusion, a third explanation
- [[nico]] ? [[social-attention]] ? L08's emotion display, now evaluated
