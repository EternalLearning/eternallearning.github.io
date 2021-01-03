---
layout: post
title: Recommendations for fixing hold violations during Pnr
---

## Pre-CTS stage
Hold timing analysis can be performed early in flow to identify hard macros which have large hold time requirements. Identifying these situations early allows planning for enough space to insert the required buffers and delay cells to fix them.

timdesign -preplace -hold

Ideally since there is no clock skew at this stage, there should be very small hold violations. If you see large hold viols, you need to understand their cause:

- due to timing constraints? For ex: large hold uncertainty value.
- related to timing model of specific cells that have large hold time requirements? for ex: like some memories.

**suggestions**
- Some memories might require a lot of buffers to fix the viols, therefore, space should be reserved to insert the buffers. (by placement blockage, cell padding, etc)
- If the viols are in the general standard cell logic, you can limit the max density. This limit applies to entire design.
- You can reserve space around specific cells by using cell padding. Make sure to delete padding prior to hold fixing.
- When floorplanning, make sure the clock pin of hard macros is easily accessible and is near the standard cell logic so that clock trees can buffer directly to the pin.

## During CTS stage
Once the clock tree is inserted and propagated, clock delay and skew are now part of timing analysis. The clock skew can cause hold timing violations and increase the number of violations. This is expected.

If useful skew is enabled during CCOpt, the skew introduced to optimize setup timing might create more hold violations.

**suggestions**
- Route the clock with a Non-Default Rule (NDR) such as "double-width double-space" to reduce SI.
- Shield the clock net to reduce SI if there is space available.
- Use upper layers for routing the clocks. Upper layers have better RC characteristics at smaller nodes.
- Reduce clock uncertainty so that it is not overly pessimistic. The clock net is routed during CTS; so actual parasitics and delays are measured accurately.
- Are the required hold margins correctly specified in the SDC?
- Are the buffers, inverters, and delay cells to be used for hold time fixing available to use?
- Using HVT cells for hold fixing allows the tool to get more delay with less leakage.


## Debugging reasons for excessive hold buffer addition:
- Unavailability of delay cells can cause Innovus to choose buffers instead of delay cells. Buffer cells have smaller propagation delay for the same area footprint.
- Check if there is a setDontUse constraint on delay cells; also verify if larger delay cells are available for Innovus to use.
- Check SDC's if there is extra OCV derating on delay cells. This may cause optDesign to avoid these cells.
- Check if HOLD critical  paths have a big clock skew. If so, fixing clock skew can help avoid a large number of buffers or inverters from being added on data paths to fix hold violations.
- Check set_clock_uncertainty is not too pessimistic, as this will make the hold violations worse.
- If using OCV, check setAnalysisMode -cppr both is set.
- To resolve congestion, use specifyCellPad or specifyInstPad  to reserve space for hold buffers, and relax them just before invoking optDesign -hold -postRoute.
