---
title: Layer-wise Relevance Propagation (LRP)
type: system
sources: [L12]
tags: [explainability, methods, learning-rule]
updated: 2026-09-21
---

# Layer-wise Relevance Propagation (LRP)

> - **Calculates the relevance of each neuron in the input layer of a NN** ?
>   importance of each input feature
> - **Utilises layer-structure and propagation mechanism of NN**
> - **Detailed analysis of the relevance of individual input features through the
>   NN**

The only XAI method in [[L12-neuro-symbolic-and-explainable-ai]] given as an
equation, and the most precise thing in the lecture.

## The rule

> `R_j` = relevance of neuron `j`
> `z_jk` = relevance contribution from neuron `j` to `k` = `a_j ? w_jk`
> `a_j` = input activation to neuron `j`, `w_jk` = weight from `j` to `k`
>
> **`R_j = ?_k ( z_jk / ?_j z_jk ) ? R_k`**

```
def lrp(net, x):
    a = net.forward_pass(x)                  # cache activations
    R = {output_neuron: a[output_neuron]}    # start with the prediction
    for layer in reversed(net.layers):       # backward pass
        for j in layer.inputs:
            R[j] = sum( (a[j]*w[j][k]) / sum(a[i]*w[i][k] for i in layer.inputs)
                        * R[k]
                        for k in layer.outputs )
    return R          # relevance per input pixel -> heatmap
```

Three passes in the diagram: **forward pass** (input image ? output `x_f`),
**relevance propagation** (backwards), **heatmap** `R_p` over the input.

## Reading the equation

The fraction `z_jk / ?_j z_jk` is neuron `j`'s **share** of everything flowing
into `k`. Each neuron's relevance is therefore its proportional contribution to
each downstream neuron, weighted by how relevant that neuron was.

The denominator normalises over **all** inputs to `k`, which gives the method its
defining property: the shares sum to 1, so

```
?_j R_j  =  ?_k R_k   at every layer
```

**relevance is conserved.** The prediction's value is redistributed backwards,
never created or destroyed, and the input heatmap sums to the output score.

> [!note] Conservation is what makes this different from a gradient
> A gradient says *how would the output change if this pixel changed* ? a
> statement about a **counterfactual**. LRP says *how much of this output came
> from this pixel* ? a statement about **decomposition** of the actual
> prediction. The first can be large where the second is zero.
>
> Conservation also means the heatmap is directly comparable across inputs,
> because everything is measured in units of the prediction. No other method in
> the lecture has a quantity that means anything.

> [!note] And it is backpropagation's structure reused for a different quantity
> [[backpropagation]] pushes **error** backwards through the same graph with the
> same local-share logic. LRP pushes **relevance**. *"Utilises layer-structure
> and propagation mechanism of NN"* is the lecture saying exactly this, and the
> point is worth stating plainly: the backward pass is a general mechanism for
> attributing a scalar at the output to the parameters and inputs that produced
> it, and error is only one thing you can attribute.

## Problems not mentioned

> [!warning] `?_j z_jk` can be zero or near zero
> Activations and weights are signed, so positive and negative contributions
> cancel and the denominator can vanish ? making relevances explode. Every
> practical LRP variant adds a stabiliser or treats positive and negative
> contributions separately [external]. The bare rule as given is numerically
> unusable.

Also unstated: how `R` is **initialised** at the output (presumably the score for
the class being explained), and how the rule applies to
[[pooling]] layers or to activations that are not ReLU.

## CAM and LRP compared

| | [[class-activation-map]] | LRP |
|---|---|---|
| Requires | a convolutional architecture | any layered network |
| Resolution | coarse ? the last feature map | **per input feature** |
| Quantity | weighted feature activation | **conserved relevance** |
| Computation | one weighted sum | a full backward pass |

LRP is the more principled and more general of the two, which is presumably why
it is the one the notes write out in full.

## See also

- [[class-activation-map]] ? [[explainable-ai]] ? [[backpropagation]]
