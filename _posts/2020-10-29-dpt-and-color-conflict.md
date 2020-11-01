---
layout: post
title: DPT and color conflict
---

Multiple patterning lithography (MPL) techniques have been used to extend the 193nm lithography to 22nm/14nm nodes. Possibly further due to the delay of extreme ultra violet lithography and electric beam lithography (EBL) Generally speaking, the MPL consists of double patterning lithography (DPL) and triple patterning lithography (TPL).

The most important issue for multiple patterning technique is how to successfully decompose the layout into several masks that can be manufactured under current 193nm optical lithography.

 When the pitch between two patterns is less than the lithography threshold, the patterns have to be separated into different masks. This process is called layout decomposition, or coloring. There are many studies on MPL layout decompositions at the mask synthesis stage to resolve the coloring conflicts, minimize the stitches, balance the mask density, or even mitigate the undesirable overlay effects.

 Meanwhile, there are studies showing that it is very important to consider the multiple patterning implications at earlier physical design stages so that the overall design and manufacturing closure can be reached.
 Both multiple patterning aware standard cell library design and multiple patterning aware physical design flow are necessary to avoid undecomposable patterns in final layout.

 The double-patterning technology (DPT) that means we can continue to use 193nm lithography to produce features at a 64nm pitch in 20nm processes is a breakthrough for manufacturing but an added complication for design. In double patterning, features that are too close together to be resolved with conventional lithography are separated onto two masks, which are exposed sequentially, with an etch step in between, to form the necessary densely packed features.

 Designers must create layouts that can be split (‘decomposed’) into two layers, a process known as coloring, after map-coloring theory. Achieving such layouts means following new design rules that encapsulate the limitations of the lithographic process and prohibit two polygons of the same color from having particular geometric relationships.


## Understand how is it handled in my design.
