---
type: lecture
lecture: L05
title: Bio-inspired Robot Sound Localisation
source: raw/lectures/BioinspiredAIWdh5.pdf
pages: 6
dates_on_pages: [2024-02-02]
tags: [audition, robotics, spiking, localisation, hybrid]
---

# L05 — Bio-inspired Robot Sound Localisation

> Source: `raw/lectures/BioinspiredAIWdh5.pdf`, 6 pages, all marked **L5**,
> dated 02.02.2024.

## Summary

The module's first **complete working system**, and the lecture where L02 finally
pays off.

L02 built spiking neurons in detail and then never used them; L03 and L04 worked
entirely with rate-coded units. L05 is the lecture where spike timing is not a
modelling choice but *the signal itself*: a sound arriving 0.25 ms earlier at one
ear than the other is a temporal quantity, and only a temporally-coded system can
read it. [[temporal-coding]] stops being an alternative and becomes a
requirement.

The lecture is organised as a single problem attacked three ways:

| | Approach | Where |
|---|---|---|
| **Biology** | [[auditory-pathway]] — MSO computes ITD, LSO computes ILD, IC integrates | p5, p6 |
| **Classical model** | [[jeffress-model]] — delay lines plus coincidence detectors | p2 |
| **Engineering** | [[cross-correlation-localisation]] + [[hybrid-acoustic-tracking]] | p2, p3, p4 |

And its punchline is that these three are **the same computation**. The Jeffress
delay-line array *is* a cross-correlator built out of neurons. This is the
strongest biology↔algorithm correspondence the module has offered — far better
supported than L03's *backpropagation ↔ plasticity* (see
[[ann-brain-correspondence]]).

## Key ideas

### The problem

**Localisation of sound sources in biological systems.** Sound has two
coordinates — see [[azimuth-and-elevation]]:

- **azimuth φ**
- **elevation δ**

**Difference in azimuth changes the time delay of the signal arriving at the
receivers.** The margin sketch shows two sources `S1`, `S2` with
`ITD(S2) > ITD(S1)`.

### The two cues

- **[[interaural-time-difference]] (ITD)** — time difference (ms) between
  arrival of the signal at the two ears.
- **[[interaural-level-difference]] (ILD)** — level difference (dB) of the
  signal at the two ears.

### [[acoustic-shadow]]

**ILD is best for high-frequency sounds** ("head shadow effect"):

- short wavelength ⇒ **high difference for high frequencies**
- long wavelength ⇒ **small difference for low frequencies** (the wave
  diffracts around the head)

⇒ **Judgement dominated by ITD in low frequencies, ILD in high frequencies.**
This is duplex theory.

- *Difference in elevation also leads to different frequency response.*

### [[jeffress-model]]

**Early Jeffress neuronal coincidence model** ([[lloyd-jeffress]]) — axons from
left and right run towards each other as **delay lines**, with **coincidence
detector** neurons arranged along them. Whichever detector fires marks the ITD,
so a *time* difference becomes a *place* code.

### [[hybrid-acoustic-tracking]]

**Inspired by the mammalian system (2-ear model, ITD).**

- **Cross-correlation** algorithm for sound localisation
- **Recurrent NN** for tracking the sound source, to improve the localisation
  task
- **Learning and adaptation to acceleration and deceleration**

Three stages:

| Stage | Block | Output |
|---|---|---|
| 1 | Correlation (ITD) → Localisation | **angle position representing location of source** |
| 2 | Tracking (SRN) | **next predicted position / angle of source** |
| 3 | Motor control | robot turns |

Inputs are two MICs producing a **digital representation of detected signals**;
`σ = # of offsets from cc`; `Θ = angle of incidence`; `ΔΘ = next position`.

### [[geometric-sound-localisation]]

```
a = c · t_ITD          (c = speed of sound)
Θ = arccos(a / b)
```

Worked example (p2): delay `d = 8` at sampling rate `r = 32000 1/s`,
`c = 340 m/s`, `b = 15 cm`:

```
a = d · c/r = 8 · 340/32000 = 0.085 m
Θ = arccos(0.085 / 0.15) = 55.48°
```

Both figures check out.

### [[cross-correlation-localisation]]

*Determine maximum similarity between two signals g(t) and h(t).* The
correlation vector represents the ITD delay between signals, which allows the
angle of incidence to be determined. Full algorithm on that page.

> **Similarity is the max dot product of shifted 0-padded inputs.
> Delay is the time difference at maximal similarity.**

That sentence connects straight back to L02 — see
[[neural-similarity-and-dot-product]].

### The SRN tracker

**Simple RNN for prediction of sound angle for a moving sound source** —
[[simple-recurrent-network]] as predictor for the next angle in the trajectory
of the source.

- Input: current sound angle. Output: predicted sound angle.
- Layer sizes written on the arrows: `45:30` input→hidden, `30:45`
  hidden→output, `30:30` hidden↔context at **1:1**.
- `t_n` → prediction → second input `t_n+1` → *now* can predict next angle
  `t_n+2`.
- ⇒ **two inputs for context needed.**

### Why hybrid?

- **Algorithmic sound source localisation is well understood algorithmically.**
- **Cross-correlation does not require training to provide azimuth angle.**
- **Neural predicting of source enables a quicker response and can learn
  temporal sequences.**

See [[hybrid-architecture]].

### [[cocktail-party-problem]]

*Humans and animals have abilities of sound localisation and sound perception in
**auditory cluttered environments**.*

### Cognitive neuroscience for robot sound localisation

**How is sound encoded?**
`ear pinna → middle ear → inner ear → auditory nerve`, giving a
**[[tonotopic-representation]]**.

**How is the encoded information processed?** → **Spiking NN**
([[spiking-neural-network]]):

- **ILD in the Lateral Superior Olive (LSO)**
- **ITD in the ~~Lateral~~ Medial Superior Olive (MSO)**
- **Integrated in the Inferior Colliculus (IC)** — *for sound localisation, a
  major centre of integration in the ascending as well as descending auditory
  pathways.*
- **Sound encoding: from sounds to spike trains** — *spikes encoding time and
  level information.*

See [[auditory-pathway]].

### The full bio-inspired pipeline (p6)

```
  L ─┐                                      Dim. reduction
     ├→ ┌──────┐ ──→ ┌────┐ ──→ ┌────────────────┐ ──→ ┌──────────────┐
  R ─┘  │ MSO  │     │ IC │     │ Classification │     │ Motor control│
        │ LSO  │     └────┘     └────────────────┘     └──────────────┘
        └──────┘
        Spiking NN                Feed-forward NN
```

⇒ **Output of MSO & LSO integrated in IC.**

### Summary (p6, verbatim)

- ITD in a **full hybrid acoustic system**
- ITD & ILD in a **hybrid spiking NN**
- Approach: **new insight into brain mechanisms**; **improve speech recognition
  by user localisation and vision**

## New pages created

Concepts — [[azimuth-and-elevation]], [[interaural-time-difference]],
[[interaural-level-difference]], [[acoustic-shadow]],
[[geometric-sound-localisation]], [[cocktail-party-problem]],
[[tonotopic-representation]], [[auditory-pathway]], [[hybrid-architecture]]

Systems — [[jeffress-model]], [[cross-correlation-localisation]],
[[hybrid-acoustic-tracking]], [[hybrid-spiking-localisation-network]]

Entities — [[lloyd-jeffress]]

## Pages updated

- [[spiking-neural-network]] — **first actual application in the module.**
- [[temporal-coding]] — ITD is the clearest case yet of information carried by
  spike timing rather than rate.
- [[simple-recurrent-network]] — used as a trajectory predictor, with concrete
  layer sizes.
- [[neural-similarity-and-dot-product]] — cross-correlation *is* the dot-product
  similarity of L02, swept over shifts.
- [[ann-brain-correspondence]] — a correspondence that actually holds.
- [[intelligent-behaviour]] — L05 satisfies L01's two requirements more cleanly
  than any lecture so far.
- [[network-architectures]] — hybrid pipelines as an architectural pattern.

## Connections

- **L02's debt is paid.** [[integrate-and-fire]], [[spike-response-model]] and
  [[temporal-coding]] were built in L02 and then left unused through two
  lectures of rate-coded networks. Sound localisation is the task that *needs*
  them: microsecond arrival differences cannot be represented by an activation
  level.
- **The Jeffress model is a place code for time.** Delay lines convert a
  temporal difference into *which neuron fires* — which makes it a close cousin
  of L01's [[place-cells]] and of the topographic maps in
  [[self-organising-map]]. Three lectures, three versions of the same trick:
  represent a continuous quantity by position in a population.
- **Hybrid is a new stance.** L03 and L04 reach for learning by default. L05
  argues explicitly that where the computation is *understood*, you should just
  implement it, and spend the learning on the part that is genuinely uncertain
  (the source's future trajectory). See [[hybrid-architecture]].
- **The correspondence is real this time.** Cross-correlation and the MSO's
  coincidence detectors compute the same function by different means. Compare
  the unsupported *backpropagation ↔ plasticity* claim on
  [[ann-brain-correspondence]].

## Unclear in the source

- **ITD given in ms.** Human ITDs max out around 0.6 ms, so sub-millisecond
  is the working range; the notes' "(ms)" is loose but not wrong. Most
  literature quotes µs. `[external]`
- **The arccos convention is not stated.** `Θ = arccos(a/b)` measures the angle
  from the **microphone axis**, not from straight ahead. The triangle sketch
  puts the right angle between `a` and the incoming ray, making `b` the
  baseline. Worth confirming against the slides — a broadside convention would
  use `arcsin`.
- **The SRN layer sizes are ambiguous.** `45:30`, `30:45`, `30:30 1:1` are
  written on arrows with no units. The reading adopted here is
  input 45 → hidden 30 → output 45, with a 30-unit context layer copied 1:1.
  Why an angle needs 45 input units is not explained — possibly 4° bins over
  180°.
- **`σ = # of offsets from cc`** is written but σ never appears again.
- **AVCN and MNTB are drawn in the p6 circuit diagrams but never defined.**
  (Anteroventral cochlear nucleus; medial nucleus of the trapezoid body.
  `[external]`) MNTB's role — sign-inverting the contralateral input so the LSO
  can subtract — is exactly what makes ILD computation work, and is absent.
- **Elevation is introduced on p1 and then dropped.** Every model in the lecture
  computes azimuth only. The one clue given, *difference in elevation also leads
  to different frequency response*, is the beginning of the spectral-cue story
  and goes nowhere.
- **The classification stage on p6 is unexplained** — a feed-forward NN mapping
  IC output to something, with no statement of its classes or training.
- **The p2 margin boxes** — *speech recognition → m = "Hello" → parse →
  response*, and *Task: 1 initial direction, 2 sound detected, 3 new direction* —
  sketch a demo scenario with no accompanying text.

See [[index]] · [[overview]] · [[log]]
