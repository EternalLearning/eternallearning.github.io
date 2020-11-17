---
layout: post
title: Latency vs skew
---

## To achieve better latency; high latency means more power dissipation
- A good placement of flip flops will reduce latency i,e., sink pins of  a clock are not placed far away, so that latency is reduced.
- Constraint the clock net routing to be on upper metal layers that are less resistive using the TopPreferredLayer and BottomPreferredLayer constructs.
- Using mesh it is possible to achieve lower insertion delays.
- Htree reduces insertion delay: the combination of larger drivers and low RC routing layers reduces the non common path clock insertion delay, potentially increasing the performance.


## To achieve better skew; high skew means closing timing might be critical for very fast designs.
- H-Tree structure is based on equalization of wire lengths. An ideal H-Tree will see same length of wire segments and similar kind of drivers from its clock root pin to the output of last level drivers. Possible to achieve low skew due to its symmetry.
- The combination of larger drivers and low RC routing layers reduces the non common path clock insertion delay, potentially increasing the performance.
