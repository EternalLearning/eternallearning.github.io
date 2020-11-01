---
layout: post
title: Deep dive into multisource cts
---

A Multisource Clock Tree System (MCTS) represents a novel clock distribution technology that fills the gap between conventional clock tree and clock mesh. Clock mesh delivers the best possible clock frequency, skew and OCV results,
and whereas conventional clock tree delivers the lowest power consumption and easiest flow.

A Multisource clock tree is a hybrid clock structure containing the best aspects of conventional clock tree and clock mesh. It offers lower skew and better on-chip variation (OCV) performance than conventional clock tree; lower clock tree area/power and shorter, easier flow compared to a clock mesh implementation.

In a clock tree, all signals radiate out in branch-like fashion from the clock source (tree root). This is why clock skew exists in clock trees. Each branch of the clock tree is made up of a unique chain of buffers and intermediate loads, and therefore exhibits delays and timing characteristics that may have little in common with those of the other branches of the clock tree.

A clock mesh is a uniform metal grid that is laid across the chip to distribute all of the clock signals, with local connections to clock loads available via a metal strap at every horizontal row or vertical column. The mesh differs from a clock tree in the way that it is driven by the incoming clock signal.

A clock mesh is driven by multiple drivers spread out across an interconnected "grid" of metal wires. The outputs of all the drivers are thus shorted together by the metal grid, ensuring that the entire clock mesh oscillates as a single entity instead of as multiple, independent branches as in a clock tree. The result is that clock skew is virtually eliminated using clock meshes and higher performance can be achieved.

Clock meshes are immune to OCV because the clock mesh is composed of nothing but metal wires, no transistors in the clock mesh means no transistor characteristics can cause variation. Clock meshes therefore improve manufacturing yield due to their immunity to OCV. Higher yield means lower prices, along with higher performance, and IC designers have long considered switching to clock meshes for just these reasons. But power consumption is more in clock meshes, since clock mesh use big grid of metal means there is a lot of capacitance to drive by the clock buffers. The clock buffers thus consume a tremendous amount of power driving the large capacitance of a clock mesh.

Clock Mesh have advantages like higher performance, reduced design engineering requirements, and immunity to OCV but due to more power consumption that can not replaced clock tree in clock distribution network.

### MCTS - MultiSource Clock Tree source
A Multisource clock tree system is a hybrid system containing the best aspects of a conventional clock tree and pure clock mesh. It offers low clock skew and better on chip variation (OCV) performance than a conventional clock tree;
lower clock tree power/area; and a shorter easier flow compared to pure clock mesh implementation.

Multisource CTS represents a novel clock-distribution technology that fills the methodology gap between conventional Clock Tree and clock mesh. Whereas clock mesh delivers the best possible clock frequency, skew, and OCV results, and
whereas conventional Clock Tree delivers the lowest power consumption and the easiest flow, multisource CTS offers a compromise between the two methods while favoring the OCV tolerant nature of pure clock mesh.

Benefits of Multisource CTS:
- Higher performance and lower skew than Conventional Clock Tree.
- Better OCV Tolerance than Conventional Clock Tree.
- Better multi-corner performance than Conventional Clock Tree.
- Less power consumption than pure clock mesh.
- Greater tolerance for irregular, highly macro density designs than pure clock mesh.
- Faster and easier flow than pure clock mesh.
- Deeper clock gating levels enabled for more complex power plan.

A multisource CTS Design comprises three different structures in the design.

A. Pre-mesh Clock Tree.

B. Multisource Mesh Fabric.

C. Moderately sized clock trees.

## A. PRE-MESH CLOCK TREE
Each buffer in the pre-mesh tree drives four other buffers, which implies that the pre-mesh topology is implemented using H-tree placement and routing. An H-tree structure provides a uniform, scalable, and predictable means of
distributing the root clock over a large area. In addition, H-trees exhibit excellent corner-to-corner variation tolerance because of their balanced structure.

## B. MULTISOURCE MESH FABRIC
The multisource mesh fabric resembles a power/ground or clock mesh fabric, but is one or two orders of magnitude less dense. The coarse fabric smoothes out any remaining clock arrival-time differences from the multiple H-tree buffers that directly drive the fabric, whereby the skew measured at the mesh plane is effectively zero.

## C. MODERATELY SIZED CLOCK TREES
The multiple clock trees attached to the coarse mesh gives the technology its name. As mentioned, designers may target the OCV performance level by targeting the depth of the clock tree. In clock mesh, the guideline is to restrict the buffer and clock-gating depth to one or, at most, two levels. Multisource CTS generally ranges from three to nine levels of buffers of clock gating. If more levels of clock gating become necessary, conventional CTS may be the natural choice.

While static timing tools alone aren’t well-suited to analyse parallel drive networks, the combination of Spice-accurate simulation with a static timing engine provides a seamless, signal-integrity-aware timing approach. Some design teams can isolate circuit-simulation activities away from pure digital design by timing the root to mesh path separately from the multiple clock trees. An ideal clock applied to the multiple clock trees enables timing and optimization in an all-digital place-and-route environment. It is always recommended, however, to perform final sign-off timing with a full circuit simulation-based timing analysis run.
