---
layout: post
title: CTS Strategies Part 1
---

In this article let us discuss in detail about different cts strategies, their advantages and disadvantages.

### Main requirements of CTS Clock structures
The main requirements for a clock tree structure are:

1) **Minimum Insertion Delay:** A clock tree with minimum insertion delay will reduce clock tree power dissipation due to few clock tree buffers, uses less routing resources.

2) **Minimum Skew:** Minimum skew helps with hold timing closure. However, a tight skew requirement will lead to increase in clock insertion delay, which in turn leads to increase in the clock power.

3) **More common paths:** Having more common paths between launch and capture flop reduces the impact of OCV effects. The variations will cancel each other when the sinks share the same clock path to the root as any process-variation occurrence in that path affects both flops equally.
4) **Low Power Dissipation:** A good clock tree structure should support implantation of clock gating to save the power.


### Traditional cts
Conventional CTS may not be optimal choice for:

- Designs with high clock frequency.
- Designs with high number of sinks.
- Designs with sinks spread all over the core area.

*Advantages:*

- Easy to implement.
- Better clock gating, allows to do clock gating at root level.
- Low power consumption.

*Disadvantages:*

- Sensitive to OCV as sinks share fewer common paths.
- Higher insertion delays.
- Difficult to achieve low skew due to asymmetric distribution of sinks.

![Traditional CTS](https://www.semiconductor-digest.com/wp-content/uploads/2020/02/F1-2.jpg)

### Mesh cts
Clock mesh structure produces lower clock skew and it is more tolerant to on-chip variations compared to conventional CTS.

In the mesh structure, there will be a network of pre-mesh drivers to drive the clock signal from clock port to input of mesh drivers. The output of all the mesh drivers will be shorted using a metal mesh, which will carry the clock signal across the block using horizontal and vertical metal stripes. Clock to the sinks will be routed from its nearest tap point from the mesh.

*Advantages:*

- Highly tolerant to OCV because of more common paths from clock root to sink pin.
- Low clock skew.
- Possible to achieve lower insertion delays.

*Disadvantages:*

- High power consumption (dynamic power) due to the parallel drivers driving the high capacitive load created by the mesh.
- Requires more routing resources to create mesh.
- Inability to use clock gating in different levels of the structure - the gating has to be performed at the local level only.
- Difficult to implement.

![Mesh CTS](https://www.semiconductor-digest.com/wp-content/uploads/2020/02/F2-3.jpg)

### H - tree
H-Tree structure is based on equalization of wire lengths. An ideal H-Tree will see same length of wire segments and similar kind of drivers from its clock root pin to the output of last level drivers. The output of last level buffers will act as tap points and the sinks will have their clock routed from the nearest tap point. It provides good OCV tolerance because of more common paths.

*Advantages:*

- Possible to achieve low skew due to its symmetry.
- Good OCV tolerance because of more common paths.
- Less power dissipation compared to clock mesh.
- Uses less routing resources compared to clock mesh.
- Some high insertion delay compared to Mesh, but lower than the conventional CTS.

*Disadvantages:*

- Need drivers with high drive strength and these drivers should be surrounded by decap cells to avoid IR drop violations.
- H-Tree wire segments should be routed with Extra care to avoid Signal Integrity issues like EM.

![H-Tree](https://www.semiconductor-digest.com/wp-content/uploads/2020/02/F3left-1.jpg)

### Multi-Source cts
Need to write about this.

### H-tree implementation:
H-Tree building is mainly divided into the following three major steps.

1) It starts with the clock root pin defined, for which we want to create conventional clock tree structure and H-Tree.
2) Placement of clock tree: In this step, high drive strength clock cells will be placed based on a given predefined location.
3) Routing of clock tree net: assign NDR and give dont touch. All routes hsld be as straight as possible to minimize skew. All routes should be in the top metal layer.

CCOPT from Anchor Point: H-Tree endpoint will be treated as an anchor point. The tool will do CCOPT from the anchor point. We will set attribute to CCOPT; so, it will balance sink of all anchor points. CCOPT will also distribute and, if needed, swap sink among all anchor point to achieve the target latency and skew.

I think we need to do stacked via to create robust h-tree, these via stacks should be dropped on output pin of super buffer.

### Conclusion

These H-Tree clock networks may be an alternative to the more traditional clock distribution networks.

The proposed clock tree optimization methodologies reduce the power dissipation without any impact on signal characteristics. The inductive behavior of the interconnects are reduced decreasing inductive noise.

In conclusion, when there is a tight skew requirement of 80~100ps and latency requirement <500ps and number of sink more than 10,000, using the H-Tree structure will be able to achieve better power, latency and skew.
