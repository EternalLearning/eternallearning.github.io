---
layout: post
title: PD Implementation Techniques
---

Assuming we have the all the input data ready.

### Floorplan & Initial Placement:

1. Analyze and understand if the design is routable.
2. Moving toward timing driven placement as routability stabilizes.
3. Adding power routing once timing and congestion converge.

The initial floorplan and placement have a primary impact on the performance of a design. Run the initial placement without any regions and guides. It is best to get a baseline placement without constraining the placer.

Early on, use the setPlaceMode -place_design_floorplan_mode true command to run placement in prototyping mode for a faster turnaround. Later, remember to set it to false otherwise cells may not be legalized.

Analyze the placement for timing and routability issues, and make necessary adjustments.

**Ensuring Routability:**

Initial iterations should focus on routability as the key to achieving predictable timing closure. You should attempt to resolve congestion before attempting to close timing.

Some guidelines to use during floorplanning and placement to avoid congestion:

- Try to understand the data flow within the block and also from chip level.
- Try different placement styles for hard macros if any like placing them on the edges, or like an island for better optimizations.
- Understand the connectivity between macros and I/O's and place them accordingly.
- Allow enough space between macros.
- Reorder scan chains.

**Validating the floorplan:**

- Nets requiring optimization should be defined as regular nets. Any specialnets will not be optimized as they will be set don't touch.
- Gaps between macros should be covered with placement blockages. And if possible remove them during optimization so that CTS/route can place buffers if necessary. Or, we could just use buffer only type blockage.
- All macros should be marked fixed, otherwise there is a chance that they might get moved during optimization.
- All the I/O terminals should be on track.
- Verify I/O PG shorts and fix them.

As the routability of floorplan stabilizes, we need to shift our focus towards placement and prects timing closure. That is timing closure with ideal clocks.

By default, preCTS timing optimization begins with the first phase in which following transforms are called: netlist simplification, high fanout net buffering (including high fanout net), multidriver buffering, DRV fixing at high level, and global optimization. After this stage, the core optimization starts for all the negative end points. While working on WNS and TNS, the software controls timing convergence by updating the design state, placement and routing, incrementally. Adaptively calling area reclaim allows you to control utilization at the preCTS stage. It also takes advantage of the upper layer characteristics by using layer assignment transform on the optimization of critical nets.

**Validating the placement:** it is important to analyze the global and local congestion to identify any areas that will be difficult to route.

- Review the overflow values in the log file.
- Turn on the congestion map and analyze any hot spots. Although the global congestion may be low, a hot spot can make the design impossible to route.
Use partial placement blockages to reduce the density in specified areas.
- There is a standalone command called congRepair that can be called in any part of the preRoute flow to attempt to relieve congestion. The command uses globalRoute + incremental placement. This can have a significantly detrimental effect on timing and often requires additional optDesign calls (and may not converge). So, use the congRepair command with caution.
- Clocks are consuming more routing resources because they are routed with wider rules, greater spacing and shielding. So it is important to model this early on by reading in the clock routing constraints prior to placement. The CTS specification file and the CCOpt property should contain the routing constraints including non-default rules (NDRs), spacing, and shield.

**Guidelines for preCTS optimization:**

perform the following sanity checks for preCTS optimization:
- Review checkDesign -all.
- Check that the SDC is clean.
- Check that the timing is met in zero wireload using timeDesign -prePlace. Violating paths in this mode are likely to be failing significantly more when actual wireloading has been applied.
- Check the don’t use report.
- Activate all required views.
- Adjust settings depending on specific scenarios: high performance (timing/power), high routing congestion, high utilization, mixture.


While performing timing optimization, check the following:
- Follow WNS/TNS convergence for each active path group.
- Check physical update – large max move of instances, large mean move of instances, routing congestion, and so on.
- Check DRV fixing convergence.
- Monitor routing congestion at different stages.

**PreCTS optimization command sequences:**

- To optimize a design after you have already run place_opt_design, use the following command:
place_opt_design -incremental.
- To perform rapid timing optimization for design prototyping, use the following commands:    
setDesignMode -flowEffort express
place_opt_design.

**Checking and debugging timing optimization results:**

- Graphically check the critical paths, not just the first one.
- Investigate cell/net delay to identify bad buffering, bad sizing, and weak cell usage.
- Investigate routing:

--> Scenic routing on standard cells area – rework on placement.

--> Check for bad routing over macros – add soft placement blockage or modify floorplan.

--> Critical paths go through congested area. Try using cell padding (using the specifyCellPad or specifyInstPad commands) or partial placement blockages (also known as density screens) to reduce the congestion – check global density and hot spot.

--> Critical path(s) go through routing congested area – review floorplan / macro placement / narrow channels.

--> Check if the worst path is going through deep logic level – If yes, see if the initial netlist is to be further improved during synthesis.


### Common types of timing and congestion problems seen during preCTS optimization and suggestions for resolving them:

- If some nets are not being optimized, run the following to output a report of the ignored nets during optimization. Use the abbreviation in the last column with the key at the bottom of the file to determine why certain nets are not being optimized.
reportIgnoredNets -outfile fileName.
- If timing after optimization is poor or degraded, check the log file for slack jumps. This can be related to physical update placement legalization/earlyGlobalRouting. In this case, check routing congestion, scaling factors, and initial placement.
- Run the following command to identify cells with a set_dont_touch or set_dont_use attribute. The file will identify these cells in the far right column. Ensure that the cells intended for optimization are available for use.
reportFootPrint -dontTouchNUse -outfile fileName.
- If similar paths are not meeting timing, create a custom path group for these paths and optimize them separately.
- If critical paths go through congested area, then try using cell padding (running the specifyCellPad or specifyInstPad commands) or partial placement blockages (also known as density screens) to reduce the congestion.
- The risk for timing optimization on designs with high-routing congestion mainly comes from nets detouring that are unpredictable. In addition to floorplan and placement recommendations made previously, it is usually beneficial to identify the weak cells and to set them as dont_use.
setDontUse cellName(s).

Some cons on creating path groups:
- Too many custom path groups may impact the run time significantly.
- Too many overlapping or nested path groups may impact TNS timing closure.

### CTS:

- Configure NDR's.
- set target max transition and target skew.
- Configure which library cells to use for buffers, inverters, and clock gates.

More can be added to this section. Will write a separate post on this.

### Routing:

After postCTS optimization, there should be few, if any, timing violations left in the design. The goals of detailed routing include the following:

- Routing the design without DRC or LVS violations.
- Routing the design without degrading timing or creating signal integrity violations.
- Using DFM techniques such as multi-cut via insertion, wire widening, and wire spacing to optimize the yield.

**Improving timing during routing:**
- Make sure the top max routing layer is set appropriately.
- If there is local or global congestion, return to placement, optimization and  postCTS optimization to optimize further until congestion is resolved.
- Check the NonDefaultRules (NDRs) and shielding and layer constraints.

** Analyze and debug of postRout optimization techniques:**
- For multi-VT designs, you can perform a LEF-Safe Optimization with only cell swapping by setting the following:
setOptMode -allowOnlyCellSwapping true
optDesign -postRoute.


### Chip finishing

Once postRoute optimization is performed, you may need to add filler cells and metal fill shapes to meet the DRC rules. The filler cells are recommended to be inserted before routeDesign, with proper setFillerMode settings. The postRoute optimization can automatically delete the fillers before optimizing, restore the deleted fillers back, and insert fillers again before eco route. In the ECO stage, deleteFiller can be used to free the placement space.

Sometimes, the library provides filler cells with built in decoupling capacitors (decap). You can also insert these fillers to improve the voltage or IR drop, normally at the expense of higher leakage currents. 

Metal fill, also called dummy metal, is used to make the metal density more uniform by adding small, floating, metal-fill shapes in empty areas. You can either use Innovus to add metal fill, or use a physical verification tool. The Innovus metal fill can meet the DRC rules for older process nodes, but for newer process nodes such as 28nm and below, the Innovus metal-fill rules are generally not sufficient and you must use physical verification tools.
