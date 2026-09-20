---
type: concept
tags: [vision, neuroscience, anatomy]
sources: [L06, L09]
status: solid
---

# The retina

The sensor at the front of the [[visual-pathway]], described in
[[L06-hierarchical-vision]].

## Layer stack

The striking fact the notes emphasise: **light enters from the back**. It passes
through the entire neural apparatus before reaching the cells that actually
detect it.

```
   ↓ light
Optic Nerve Fibers
Ganglion Cells
Inner Synaptic Layer
Cells — Amacrine, Bipolar, Horizontal
Outer Synaptic Layer
Receptor Nuclei
Receptors — Pigmented Layer
```

Output leaves via the **ganglion cells**, at the top — so the signal travels
*back out* the way the light came in. The retina is wired backwards, which is
exactly the sort of fact that makes the "the brain is an optimal design" reading
of bio-inspiration harder to sustain.

## Photoreceptors

| | **Cone** | **Rod** |
|---|---|---|
| Vision | **colour** | **night** — "almost entirely responsible" |
| Location | at the **fovea** | **outer edge** of retina |
| Light level | functions best in **bright light** | low light |

Both share the same three-part anatomy:

- **synaptic ending** — output to the next layer
- **inner segment** — cell machinery
- **outer segment** — contains the **photopigment** that absorbs light

## Fields of view

The lecture opens with **different visual fields of view in nature**: a rabbit,
whose laterally-placed eyes give near-panoramic coverage with little overlap,
versus a human, whose forward-facing eyes give a narrow but **binocular** field.

The conclusion drawn: `eg:` the human brain is more complex, because it must
**integrate information from both eyes**. Overlap is expensive — it buys depth
at the cost of coverage, and requires machinery to fuse the two images.

## Pseudocode — the ON-centre ganglion response

Each ganglion cell computes a difference of two pooled regions. See
[[receptive-field]] for what the regions are.

```
function ganglion_ON(image, x, y, r_centre, r_surround):
    centre   = mean(image over disc of radius r_centre   at (x,y))
    surround = mean(image over annulus r_centre..r_surround at (x,y))
    drive    = centre - surround          # ON: centre excites, surround inhibits
    return firing_rate(drive)             # rate rises with stimulus strength

function ganglion_OFF(image, x, y, r_centre, r_surround):
    return firing_rate(surround - centre) # signs reversed
```

Because `drive` subtracts the surround from the centre, **uniform illumination
gives zero** — the retina throws away absolute brightness and transmits only
*change*. This is the same zero-sum structure as the horizontal-line kernel in
[[convolutional-network]], whose weights `[−1,−1,−1; 2,2,2; −1,−1,−1]` also sum
to zero.

## Not covered in the source

**Amacrine, bipolar and horizontal cells** are named in the layer stack and
never described. Their role in constructing the centre-surround
[[receptive-field]] — horizontal cells providing the lateral inhibition that
makes the surround — is not mentioned. `status: stub-by-source` for those three.

## Related

[[visual-pathway]] · [[receptive-field]] · [[action-potential]] ·
[[excitatory-and-inhibitory-neurons]] · [[L06-hierarchical-vision]]


## Centre–surround, one level up (L09)

[[saliency-model]] computes **centre–surround differences** over *feature* maps
— colour, orientation — rather than over luminance, and then normalises.

That is this page's receptive-field organisation applied to a different input
space, and it explains [[pop-out-effect|pop-out]]: an element identical to its
neighbours is suppressed, an element unlike them survives. The retina does it to
luminance so that uniform illumination costs no spikes; the saliency model does
it to features so that uniform texture costs no attention.

Same operation, same motivation — **code the difference, discard the constant**
— twice in the module, three lectures apart, never linked.
