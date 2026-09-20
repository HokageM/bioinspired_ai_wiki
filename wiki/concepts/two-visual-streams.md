---
type: concept
tags: [vision, neuroscience, anatomy, contradiction]
sources: [L06, L04]
status: solid
---

# Two visual streams (ventral and dorsal)

**Higher visual processing** — the *dichotomy of object processing* in the
occipital lobe, from [[L06-hierarchical-vision]], p4–p5.

| Stream | Projects to | Encodes | Question |
|---|---|---|---|
| **Ventral** | **temporal cortex** | shape, colour, texture | **What?** |
| **Dorsal** | **parietal cortex** | object's spatial information | **Where?** |

- **Partitioning found by [[leslie-ungerleider]].**
- **Separation underpinned by lesion studies** — damage to one stream impairs
  one capacity and spares the other, which is what makes this a *dissociation*
  rather than a correlation.
- The dorsal stream is *also sometimes referred to as the **how**-path, due to
  sensory-motor transformation* — because knowing *where* something is, is
  chiefly useful for acting on it. Grasping an object requires its position and
  orientation, not its identity.

## This corrects L04

> [!important] A contradiction inside the module, now resolved
> [[L04-embodied-language-processing]] presented a dual-stream account of
> **language** in which *comprehension* (sound → meaning) was labelled
> **dorsal** and *production* **ventral**.
>
> L06 states plainly: **ventral = what, dorsal = where.** Meaning is *what*.
> So L04's labels are **swapped**.
>
> This was flagged on [[dual-stream-hypothesis]] at L04 ingest time as a
> suspected error against external reference. It is now confirmed **by the
> module's own later lecture**, which is a stronger result: the two lectures
> disagree with each other, and L06 is the one that matches the literature.

The underlying anatomy is the same in both cases — ventral runs forward along
the temporal lobe, dorsal runs up into the parietal lobe — so the language
version in [[dual-stream-hypothesis]] is an *adaptation* of the visual finding
described here, not an independent one. The visual version is the original.

## `eg:` A neural network for the two streams

The lecture's proposed experiment, given as a sketch with no result:

> *How about storing multiple sensory sequences in one RNN? Can the sensory info
> still be encoded in two streams?*

The diagram shows a shared input `μ` plus additional inputs feeding into
separate **ventral layers** and **dorsal layers**, which then converge on a
common output.

```
# The architecture the sketch implies
function two_stream_rnn(sequence):
    h = shared_recurrent_state
    for x in sequence:
        h       = recurrent_update(h, x)
        ventral = f_ventral(h)        # trained toward object identity — "what"
        dorsal  = f_dorsal(h)         # trained toward spatial info   — "where"
    return combine(ventral, dorsal)

# The open question: with a SHARED h, do the two heads actually
# specialise, or does the recurrent state entangle them?
```

Both questions it poses are left unanswered. The interesting one is the second:
a dissociation in the brain is demonstrated by **lesion studies**, so the
matching test for the network is an ablation — knock out the ventral layer and
see whether *what* degrades while *where* survives. The notes do not propose
this. `[external]`

## Why the module keeps returning to this

Three lectures now split processing into parallel specialised pathways:

- **L04** — dual streams for language (comprehension / production)
- **L05** — the [[auditory-pathway]] splits [[interaural-time-difference]] and
  [[interaural-level-difference]] into separate brainstem nuclei
- **L06** — ventral and dorsal visual streams

The consistent claim is that brains do **not** compute one general-purpose
representation and query it. They commit early to separate pathways per
question, and the pathways are anatomically distinct enough to be lesioned
independently. This sits awkwardly beside the module's enthusiasm for single
end-to-end networks with one shared hidden state — which is exactly the tension
the p5 sketch is groping at.

## Unclear in the source

- The notes write "**Ungerleider**" with no first name, date, or co-author.
  `[external]` The 1982 partitioning is standardly credited to Ungerleider **and
  Mishkin**; Mishkin is not mentioned.
- No lesion study is named or described, despite being cited as the evidential
  basis.
- **V4** and **V5** from the p1 area list are never connected to the two
  streams, though the mapping (V4 → ventral, V5 → dorsal) is the obvious one.
- The *how*-path reading is mentioned in one clause and not developed.

## Related

[[dual-stream-hypothesis]] · [[visual-pathway]] · [[leslie-ungerleider]] ·
[[auditory-pathway]] · [[recurrent-neural-network]] ·
[[language-areas-of-the-brain]] · [[ann-brain-correspondence]] ·
[[L06-hierarchical-vision]] · [[L04-embodied-language-processing]]
