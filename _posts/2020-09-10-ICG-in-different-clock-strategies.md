---
layout: post
title: Optimizations in Physical synthesis
---

In this article we shall discuss about different optimization techniques that could be done in synthesis for timing correction.

1) Gate sizing: Gates with larger size has lower output resistance and gates with smalled size has more output resistance.

2) Netlist restructuring: Only changes existing gates, does not change functionality. Chnages inclue

- cloning: duplicating gates. Cloning reduces the fanout capacitance and reduce the overall delay. 
- Redesign of fanin/fanout tree.
- swapping communicative pins.
- gate decomposition - changing AND-OR to NAND-NAND.
- Boolean restructuring.
