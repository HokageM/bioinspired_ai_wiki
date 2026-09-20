# Bio-Inspired AI Wiki — Schema

This repository is an **LLM-maintained personal wiki** about bio-inspired AI:
neuroscience, evolution, swarm/collective intelligence, and their machine-learning
analogues.

**Division of labour:** the human curates sources, asks questions, and directs
emphasis. The LLM writes and maintains *every* file under `wiki/`. The human does
not hand-edit wiki pages; if a page is wrong, tell the LLM and it fixes it.

---

## 1. Layers

| Layer | Path | Owner | Mutability |
|---|---|---|---|
| Raw sources | `raw/` | Human | **Immutable** — never edit or delete |
| Wiki | `wiki/` | LLM | LLM writes freely |
| Schema | `CLAUDE.md` | Both | Co-evolved; update when conventions change |

### Primary source type
The main sources are **the human's own summaries of lectures 1–13 of the
Bio-Inspired AI module**, as **scanned handwritten notes in PDF form**
(`raw/lectures/BioinspiredAIWdh<N>.pdf`). They have **no text layer** — they must
be rendered to images and read visually (see §2.1). These are first-person course
notes, not published papers. Treat them as:
- **Authoritative for module scope** — what the module covers and how it frames things.
- **Not authoritative for facts** — they are summaries, so they may be lossy,
  compressed, or contain the note-taker's misunderstandings.
- When a lecture note is vague, ambiguous, or clearly abbreviated, **flag it**
  rather than silently inventing detail (see §6 Uncertainty).

Later sources (papers, articles, textbook chapters) may be added. They are
ingested identically but live in a different `source_type`.

---

## 2. Directory layout

```
raw/                     # immutable sources
  lectures/              # BioinspiredAIWdh<N>.pdf — scanned handwritten notes
  papers/                # optional, later
  articles/              # optional, later
  assets/                # images referenced by sources

wiki/
  index.md               # content catalogue — the map of the wiki
  log.md                 # chronological append-only record
  overview.md            # the module in one page: arc, themes, structure
  lectures/              # one page per lecture:  L01-<slug>.md
  concepts/              # ideas, mechanisms, principles: <slug>.md
  entities/              # people, organisms, named systems, datasets: <slug>.md
  systems/               # concrete algorithms & architectures: <slug>.md
  syntheses/             # cross-cutting analyses, comparisons, query outputs
```

### 2.1 Reading a scanned lecture PDF

The PDFs are photographs of handwritten pages. `pypdf` text extraction returns
almost nothing — do not rely on it. Render and read visually:

```python
import pymupdf, os
src = r"raw\lectures\BioinspiredAIWdh3.pdf"
out = os.path.join(os.environ["TEMP"], "wiki_render")
os.makedirs(out, exist_ok=True)
for i, pg in enumerate(pymupdf.open(src)):
    pix = pg.get_pixmap(matrix=pymupdf.Matrix(2.4, 2.4))   # ~1430x2020, ~350 KB
    pix.pil_save(os.path.join(out, f"j{i+1:02d}.jpg"),
                 format="JPEG", quality=72, optimize=True)
```

Then view each `j*.jpg` in page order. Requires `pymupdf` and `pillow`
(`py -m pip install pymupdf pillow`); the interpreter is `py`, not `python`.
Render to `%TEMP%`, **not** into the repo. Delete renders when done.

**File-to-lecture mapping:** the filenames are `Wdh` (*Wiederholung*, revision)
numbers, not always 1:1 with lectures — `BioinspiredAIWdh1und2.pdf` covers L01
*and* L02. Check the blue `L<n>` section markers inside the pages, which is where
the human marks the lecture boundary. Cite by the marker, not by the filename.

**Reading the notes:** highlighter colour is signal. Blue box = lecture marker.
Green = definition or key mechanism. Yellow = formula or term to memorise.
Orange = a caveat, a problem, or an example (`eg:`). Marginal doodles are
mnemonics. Preserve every formula exactly as written, then comment on it.

**Naming:** lowercase kebab-case slugs (`ant-colony-optimisation.md`,
`hebbian-learning.md`). Lecture pages use `L01-`…`L13-` prefixes so they sort.
One concept per page. Prefer a new page over a long section in an existing page.

**Concept vs. system vs. entity — the rule of thumb:**
- *Concept* = an idea or principle you could explain on a whiteboard
  (stigmergy, emergence, credit assignment, exploration–exploitation).
- *System* = something you could implement and run
  (PSO, NEAT, ACO, spiking neural network, CPPN).
- *Entity* = a named thing in the world
  (Marco Dorigo, Ken Stanley, honeybee, C. elegans, Braitenberg vehicle).

---

## 3. Page format

Every wiki page starts with YAML frontmatter:

```yaml
---
title: Ant Colony Optimisation
type: system            # lecture | concept | system | entity | synthesis | overview
tags: [swarm, optimisation, stigmergy]
sources: [L04, L09]     # lecture IDs / source slugs this page draws on
created: 2026-09-20
updated: 2026-09-20
status: stub            # stub | developing | solid
---
```

`status` meaning:
- `stub` — exists so links resolve; a definition and little else.
- `developing` — real content, but known gaps.
- `solid` — covers what the sources say; would survive a lint pass.

Body structure (adapt as needed, keep the spirit):

1. **One-sentence definition.** What this is, plainly.
2. **Biological origin.** What in nature this comes from, if applicable. This is
   the module's whole point — always try to fill it in.
3. **Computational form.** How it is turned into an algorithm/architecture.
   Include the equations exactly as the lecture gives them, **and pseudocode
   wherever the content is algorithmic at all** — a learning rule, an update
   equation, a forward pass, a simulation loop. This is a standing instruction,
   not an option. Use fenced blocks, name variables as the lecture does, and
   make the block runnable-in-spirit (loop structure, initialisation, update,
   termination). If the lecture gives an equation but no algorithm, write the
   smallest loop that would apply that equation and mark any step you had to
   supply as `# [external]`.
4. **Where it appears in the module.** Link the lecture pages.
5. **Relations.** `## See also` with `[[wikilinks]]` — contrasts, prerequisites,
   descendants.
6. **Open questions / gaps.** What the sources leave unanswered.

### Linking
Use Obsidian-style `[[wikilinks]]` for internal links (the vault root is the
repo root, so `[[ant-colony-optimisation]]` resolves by filename).
**Every page must have at least one inbound and one outbound link.** When you
create a page, immediately add the inbound link from wherever it belongs.

### Citations
Cite the lecture, not vaguely. Inline: `(L07)`. If the lecture summary is
paraphrased closely, quote it: `> "…" (L07)`. Never attribute a claim to a
lecture that does not contain it.

---

## 4. Lecture pages

`wiki/lectures/L07-<slug>.md` is the canonical record of one lecture. Structure:

```markdown
## Summary
3–6 sentences: what this lecture was about and why it sits here in the module.

## Key ideas
Bulleted, each linking to its concept/system page.

## New pages created
## Pages updated

## Connections
How this lecture builds on earlier ones and sets up later ones.

## Unclear in the source
Things the summary left ambiguous — candidates for follow-up.
```

Lecture pages are the **bridge between `raw/` and the rest of the wiki**: they are
the only pages that map 1:1 onto a source. Everything else is synthesis.

---

## 5. Operations

### INGEST — "ingest lecture N"
Default is **one source at a time, human in the loop**.

1. Read the source in `raw/lectures/` in full.
2. **Discuss first.** Report key takeaways, what's new vs. what repeats, and
   which pages you propose to create/update. Wait for direction unless told to
   proceed autonomously.
3. Write/update `wiki/lectures/LNN-<slug>.md`.
4. Create or update concept/system/entity pages. **Update, don't duplicate** —
   always search `wiki/` for an existing page before creating one.
5. Integrate, don't append: if new material refines an existing claim, rewrite
   the claim. If it *contradicts* one, keep both and flag it (§6).
6. Fix cross-references in both directions.
7. Update `wiki/index.md` and `wiki/overview.md` if the module arc has shifted.
8. Append to `wiki/log.md`.

A single lecture typically touches 8–15 pages. That is normal and expected.

### QUERY — asking questions
1. Read `wiki/index.md` first, then drill into the relevant pages.
2. Answer from the wiki, with citations to lecture IDs.
3. If the wiki can't answer it, say so plainly — do **not** fill the gap with
   general knowledge presented as if it came from the module. If you add outside
   knowledge, mark it `[external]`.
4. **File good answers back.** If an answer is a genuine synthesis (a comparison,
   a connection, an argument), offer to save it to `wiki/syntheses/` as a page
   with `type: synthesis` and `sources: [query]`. Explorations should compound.

### LINT — "lint the wiki"
Report, don't silently fix. Check for:
- Contradictions between pages.
- Orphan pages (no inbound links) and dead `[[links]]` to nonexistent pages.
- Stub pages that have been stubs for several ingests.
- Concepts named repeatedly across pages but with no page of their own.
- Missing biological-origin sections (the module's core framing).
- `sources:` frontmatter that has drifted from the actual citations in the body.
- Gaps a targeted search or an extra source would fill.
Finish with: **suggested next questions** and **suggested sources to find**.

---

## 6. Uncertainty, contradiction, and honesty

These rules matter more than completeness.

- **Never invent.** If a lecture summary mentions a term without explaining it,
  create a `stub` page saying exactly that: "Mentioned in L06 without definition."
- **Distinguish module content from world knowledge.** Anything not traceable to
  a source gets marked `[external]` inline.
- **Contradictions get a `> [!warning]` callout** on both pages, naming both
  sources and what each claims. Do not quietly pick a winner.
- **Note when the note-taker seems uncertain.** These are the human's own
  summaries; hedging in the source ("I think", "something about") is signal —
  surface it in the lecture page's "Unclear in the source" section.

---

## 7. index.md and log.md

**`wiki/index.md`** — content-oriented catalogue, grouped by
Lectures / Concepts / Systems / Entities / Syntheses. One line per page:

```markdown
- [[ant-colony-optimisation]] — pheromone-trail optimisation from foraging ants. `L04, L09` · solid
```

Keep it sorted within groups. Update on every ingest.

**`wiki/log.md`** — append-only, newest at the bottom. Every entry starts with a
consistent parseable header:

```markdown
## [2026-09-20] ingest | L04 Swarm Intelligence
Created: ant-colony-optimisation, stigmergy, emergence
Updated: index, overview, L03, self-organisation
Notes: ACO framing here conflicts with the optimisation framing in L02 — flagged.
```

Types: `ingest`, `query`, `lint`, `refactor`. So `grep "^## \[" wiki/log.md | tail -5`
gives recent history.

---

## 8. Conventions

- **Language:** British spelling (`optimisation`, `behaviour`) — match the module.
- **Tone:** explanatory and precise. Write for the human reading this in six
  months who has forgotten the lecture.
- **Maths:** LaTeX in `$…$` / `$$…$$`.
- **No hedging filler.** "It is worth noting that" adds nothing.
- **Tags** are a small controlled vocabulary. Reuse existing tags before minting
  new ones; keep the live list in `wiki/index.md`.
- **Git:** commit after each ingest, message `ingest: L04 swarm intelligence`.

---

## 9. Current state

`raw/lectures/` holds `BioinspiredAIWdh1und2.pdf` … `Wdh13.pdf` (12 files;
`Wdh9` **is** present — an earlier note to the contrary was wrong).
**Ingested so far: L01, L02, L03, L04, L05, L06, L07, L08, L09, L10, L11, L12, L13. **All 13 lectures are ingested; the source set is complete.** See [[log]] for history and
[[index]] for the page catalogue. [[overview]] carries the provisional map of
the module arc and is rewritten as each lecture lands.

Remaining: none.

**Watch for as later lectures land:** evolution and swarm/collective
intelligence are named in the module's scope and have not appeared at all
through L13. **Evolution arrived at L11**; **swarm and collective intelligence never appear at all — this gap is now permanent for this source set.** Likewise reinforcement learning (14 named-only appearances) and "transformer" (4 systems, never defined). If they arrive, expect new top-level groupings in `index.md` and a
new tag cluster (`evolution`, `swarm`, `optimisation`, `artificial-life`).
