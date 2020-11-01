---
layout: post
title: Difference between CTS, MultiSource CTS and Mesh
---

There are four key differences between conventional CTS, multisource CTS, and clock mesh: shared path, mesh fabric, design complexity, and timing analysis. Each subsequent section discusses each of the three clock distribution methods with respect to these key differences.

## Shared path
**Conventional CTS**
- Conventional CTS is characterized by an organic tree structure from the clock root that branches out to each of the sinks in the design. There is unlimited depth for both buffer and clock-gating levels. Most of the sinks in the design share very few paths back to the clock root—so few, in fact, that for any two sinks in the design, the only reliably shared part of the path is the root buffer.

**MultiSource CTS**
- Multiple clock trees of small to moderate depth primarily distinguish multisource CTS.There are typically between three and nine levels of clock gating and buffers. The multiple clock trees are at the bottom of the structure below the mesh grid and all of the structure above the mesh form a shared path back to the clock root. A substantial portion of the overall insertion delay of the clock is in the form of a shared path.

**Clock Mesh**
- Clock Mesh is characterized by an extremely shallow logic depth below the mesh, usually just a single buffer or clock gate directly driving the sinks. Most of the insertion delay in a clock mesh design is a large, shared path from the root to the mesh.

***Verdict on shared path - clock mesh exhibits the best clock frequency performance and best OCV tolerance, conventional CTS delivering good to very good clock frequency, moderate OCV performance and multisource CTS comes in the middle of conventional CTS and clock mesh.***

## mesh fabric
**Conventional CTS**
- No such concept as mesh fabric.

**MultiSource CTS**
- Not as dense as clock mesh.

**Clock mesh**
- Very dense mesh fabric.

## design complexity
***Conventional CTS is pretty straight forward, and clock mesh on the other hand is at extreme case. Same goes with timing complexity.***

## Power trade off
Conventional CTS has lot of knobs to play with power, since multisource CTS has less dense clock mesh, the power is less compared to clock mesh. Clock mesh consumes lot of power. It is a design trade off between performance and power.

***The three technologies explored in this article cover the polar extremes with conventional CTS delivering good to very good clock frequency, moderate OCV performance, best low-power profile, and best ease of use. On the other extreme, clock mesh exhibits the best clock frequency performance, the best OCV tolerance, the worst low-power profile, and the lowest ease of use.***
