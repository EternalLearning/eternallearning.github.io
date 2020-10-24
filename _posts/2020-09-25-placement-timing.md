---
layout: post
title: Placement and timing
---

There are 5 stages that happen in place_opt.
1) initial placement: Perform wire length driven coarse placement.

2) HFN buffering: Removes buffer trees, perfoms hfn synthesis and logic drc fixing.

3) Init optimization: performs quick timing optimization.

4) Final Placement: performs incremental and final timing driven placement to improve timing and routability. Global routing used for congestion removal.

5) Final optimization performs final full scale optimization and legalises the design.

### Port Punching:
Port punching is nothing but duplicate ports. This changes the netlist but reduces the number of buffers added. To disable port punching on specified cells we can do set_freezE_ports *

**Why we need to create scenarios?**
Concurrent MCMM optimization requires creation of scenario.
- Optimization occurs in active scenarios , for enabled analysis type.


** Distribution of spare cells**
- Spare cells can be included in the synthesis netlist.
- spare cells will be placed along with regular standard cells, and automatically spread out evenly. place.coars.enable_spare_cell_placement true. When false , spare cells are still placed but not spread out evenly.
- is_spare attribute is true.

Cells are identified as spare cells if:
- all inputs are tied high or low. The spare cells inputs can be connected to high or low logic levels. To prevent the gate from oscillating ( create noise and consuming power), tie the inputs to ground or power, as appropriate.
- All outputs are floating.

Automatic spare cell placement work only for spare cells that have  all inputs tied low or high and have output ports that float. Addtionally, due to the current tool limitations, you must set the attribute is_spare_cell on the spare cells to enable the tool to place spare cells automatically. Any spare cells that are connected to signal nets, such as sequential cells connected to a clock, cannot be distributed automatically. You need to insert these spare cells manually. **Check how this is in my design**.

Ideal network constraints on high fanout nets like set/reset/enable prevent buffering during placement - which is not desired. Remove all ideal network constraints to allow HFS during placement.

Every lib cell can be restricted for certain optimization tasks using lib cell purpose. "power, hold, cts, max tran, max cap".

Big driver cells have EM issues. ulvt registers have high leakage.

**Dynamic power optimization** - requires switching activity file, SAIF.
For FinFet technologies, leakage and dynamic power may trend differently within a vt class (different channel lengths)

leakage : xlvt-s> xlvt-l (l-long, s-short)
dynamic: xlvt-s < xlvt-l

**Why would you want to exclude certain cells from being used during optimization?**
- example, exclude high drive cells (to avoid potential EM or crosstalk problem).
- high pin count to reduce congestion.
- ulvt registers to reduce leakage power without scarificing critical timing.

**Some efforts/optimizations for placement/congestion**
- Increase placement and congestion effort to high, there might be more run time now, also may affect timing.
- Use global route based HFS and drc fixing so that we will have results more close to reality. This will help esp for macro dominated blocks.
- Cell and pin density analysis and use keepout margins and placement blockages accordingly.
- If cell density hotspot coincides with congestion density hotspot, then lower the cell density in that area using congestiom driven max utilization app option.
- consider congestion for each layer separately, congestion_layer_aware true.
- Optimising pin access. Enable pin optimization during leagalization(improves the routability of areas with high pin density by redistributing the cells)
- congestion driven restructuring, this improves wire length and routability.

**Some efforts/optimization for placement/timing**
- set timing opt effort application option medium or high at the expense of runtime, area and possibly power increment.
- Stream legalization targets smaller move in order to avoid few large moves. Overall, original illegal cell stays, other smaller cells around it are moved slightly.
- create bounds or regions to help tool place timing connected paths close.
- Improve timing driven placement results by enabling auto timing control, whne this is enabled, it enabled tn_driven placement which focusses on all violating timing endpoints instead of worst target wns(which is default).
- icg splitting and clock aware placement for timing critical paths to the EN of icg. (optimize icgs). Trail CTS enables accurate identifiaction of critical ICG enable timing paths - requires complete/accurate CTS setup. Uusually ICG have auto bound true so that they are clustered together with their logically connected previous and next stages. These are placed based on fanout instead of EN timing. 

**Some effort/optimization for placement/power-area**
- increase power and area optimization effort levels.
With power high effort, we may also see worsen TNS and/or area.
With area high effort, we may see worsen TNS, bcz no duplication of cells, that can lead to one cell driving high fanout meaning more delay.
- enable pre-route layer optimization.
Automatically assigns longer/more timing critical nets to higher layers using min/max layer constraint. place*opt*layer true. You cam control which net gets promoted using opt*layer*critical*range , by default 0.7 which means that nets with a slack in the range of WNS - WNS*0.7 will be considered for promotion.
- enable global route-based layer binning. This can be achieved by enabling route aware estimation. If this is set this works on all nets unlike pre -route layer optimization.
- enable low-power placement. This needs saif.Reduces dynamic power by moving cells to shorten high switching activity signal nets.

Across all path groups, final placement focuses on reducing or maintaining the aggrgated weighted wns. Apply more weights on certain path groups to effectively optimize timing.
