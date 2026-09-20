---
title: "L01 — Introduction to Bio-Inspired AI"
type: lecture
tags: [foundations, navigation, agents]
sources: [L01]
source_file: raw/lectures/BioinspiredAIWdh1und2.pdf
lecture_date: 2024-01-18
created: 2026-09-20
updated: 2026-09-20
status: solid
---

# L01 — Introduction to Bio-Inspired AI

## Summary

A short framing lecture that sets up the entire module. It defines what
[[intelligent-behaviour]] means for an artificial agent, states the two
requirements that make an agent *bio-inspired* rather than merely effective, and
gives one worked example from spatial navigation — [[place-cells]] and
[[grid-cells]] combining into an inner positioning system. It closes with the
thesis that carries the whole module: bio-inspired AI lets us solve problems
*informed by the best problem solvers in nature*. Everything that follows —
[[spiking-neural-network]], evolutionary methods, swarm methods — is an instance
of that thesis.

## Key ideas

- **[[intelligent-behaviour]]** — five capabilities a bio-inspired artificial
  agent should show: make decisions; learn and develop; communicate and
  cooperate; react to something new; interpret images and scenes (L01).
- **Two requirements** for calling an approach bio-inspired (L01):
  1. Consider findings about intelligent behaviour in nature.
  2. Learn, represent and process based on bio-inspired principles.

  The second is the sharper one: it is not enough to be *motivated* by biology,
  the representation and the processing must follow the biological principle.
- **[[place-cells]]** — an inner map of the environment; each cell is active in
  a particular place (L01).
- **[[grid-cells]]** — form a coordinate system for navigation (L01).
- **Combination** — place cells and grid cells together give a *comprehensive
  inner positioning system* (L01). This is the lecture's model example of the
  pattern the module repeats: biology solves a problem with a mechanism, we
  identify the mechanism, we reimplement it.
- **Thesis** — "Bio-inspired AI allows to solve problems informed by the best
  problem solvers in nature" (L01).

## New pages created

[[intelligent-behaviour]], [[place-cells]], [[grid-cells]]

## Pages updated

[[index]], [[overview]]

## Connections

L01 is pure framing and introduces no computational machinery. The example it
chooses — navigation cells — is *not* followed up in [[L02-spiking-neural-networks]],
which goes straight to neuron-level modelling. The unifying thread is the
**level-of-abstraction question**: L01 points at a systems-level mechanism
(cells that encode space), L02 drops to the single-neuron level. See
[[overview]] for how the module's levels stack.

## Unclear in the source

- The grid-cell line is abbreviated in the notes and hard to read — it reads
  approximately "reaching for / covering certain locations". The standard
  account is a periodic hexagonal firing lattice, but the notes do not say
  "hexagonal", so [[grid-cells]] records only what is legible.
- No lecturer, textbook or reference is named beyond the initials "KV" in the
  page header. The module has no cited literature so far.
- The five capabilities are listed without explanation of why *these* five.
