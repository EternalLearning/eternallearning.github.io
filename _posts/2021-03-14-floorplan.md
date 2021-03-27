---
layout: post
title: Floorplan basics
---

Floorplanning a chip or block is an important task of PD in which location, size and shape of soft modules, and placement of hard macros are decided. Floorplanning sometimes can also include I/O pad, pin placement, bump assignment, bus planning, power planning and more.

Common floorplan sequence:
1. importing design.
2. understanding design connectivity.
3. running placement and early global route to view placement and routing congestion.

- calculating density: core size - std cell area + macro area + halo / core utilization.
- configuring standard cell rows.
- grouping instances.
- power domain if any.
- 
