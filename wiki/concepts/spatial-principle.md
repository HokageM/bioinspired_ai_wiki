---
type: concept
tags: [multimodal, neuroscience]
sources: [L07]
status: solid
---

# Spatial principle (enhancement and depression)

The first of two measured signatures of [[superior-colliculus]] neurons in
[[L07-crossmodal-processing]] p4. The second is [[inverse-effectiveness]].

| Condition | SC response |
|---|---|
| auditory only | baseline |
| visual only | baseline |
| **enhancement** | **large** |
| **depression** | **small ? below either unimodal response** |

- **Enhancement:** visual and auditory stimulus **close in time and space**
- **Depression:** visual and auditory stimulus **displaced from one another**

Restated probabilistically on p5:

- **Depression:** *more probable response to only-visual than response to both
  (crossmodal).*
- **Enhancement:** *more probable response to cross-modal than to only-visual.*

## Why it is not a rule but a consequence

Depression is the interesting half. A naive integrator would treat two stimuli as
*more* evidence than one, and respond more. The SC responds **less**.

That makes sense once you take the columnar architecture of the
[[superior-colliculus]] seriously. A column codes a *position*. Two stimuli in
the same place are evidence about one event and belong in one column. Two stimuli
in different places are evidence about **two different events** ? and the SC's
job is to pick one thing to orient towards, not to average them. Depression is
lateral inhibition doing exactly what it should: suppressing a column whose
evidence is contradicted by a competing location.

So the spatial principle is the [[unity-assumption]] implemented in tissue.
Consistent inputs unify and enhance; inconsistent inputs segregate and compete.
The lecture states both facts and does not connect them.

## Pseudocode

```
function sc_column_response(column, visual, auditory):
    v = visual[column]
    a = auditory[column]
    drive = v + a                                   # co-located ? enhancement
    for other in columns != column:
        drive -= k * (visual[other] + auditory[other])   # displaced ? depression
    return rectify(drive)
```

```
# The three regimes the bar chart shows, in one line each:
#   auditory only   : drive = a
#   visual only     : drive = v
#   enhancement     : drive = v + a                      (same column)
#   depression      : drive = v - k*a                    (different columns)
```

The subtraction is the whole content of the principle, and it is the same
centre-surround arithmetic as the retinal [[receptive-field]] in L06 ? excitation
at the locus, inhibition around it. The module builds the same circuit twice, in
two structures, and does not remark on it.

## Unclear in the source

- **"Close in time and space"** ? no window is given for either. Both have
  measured values in the literature and neither appears here.
- **No mechanism** is proposed. Lateral inhibition is the obvious candidate and
  the notes do not name it. `[external]`
- **Time and space are conflated.** Enhancement requires proximity in *both*, but
  the principle is called *spatial* and temporal proximity is never separately
  discussed, despite temporal ventriloquism appearing two pages earlier.

## Related

[[superior-colliculus]] ? [[inverse-effectiveness]] ? [[unity-assumption]] ?
[[multisensory-integration]] ? [[receptive-field]] ?
[[excitatory-and-inhibitory-neurons]] ? [[histogram-based-som]] ?
[[L07-crossmodal-processing]]
