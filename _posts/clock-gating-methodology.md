---
layout: post
title: Clock Gating Methodology for power and timing QoR
---

**Benefits of Clock Gating:**
1) Dynamic Power savings
2) Eliminating multiplexers saves area. In earlier designs we used to use multiplexer for each register which consumes lot of area if we have to implement multiplexer for every register.
3) Easy to implement - no RTL code change is required. Clock gating is automatically inserted by the tool. Technology independant.

**Clock gating methodology during RTL synthesis**
set_clock_gating_style - sets the clock gating style
read_verilog - read in verilog
create_clock - define the clocks
insert_clock_gating - insert clock gating
compile - compile

Clock gating options for set_clock_gating_style:
1) Maximum fanout - By default, the fanout is unlimited. This value is the max fanout of each clock gating element.
2) Minimum bitwidth - This is the min bitwidth of register banks that will be gated. Default is 3.

Clock gating options for insert_clock_gating:
The -global option looks across hierarchical boundaries fort he common enable.

Measure the quality of inserted clock gating:
report_power
report_clock_gating

Integrated, latch-based, clock gate (ICG) is recommended.

Difference between integrated clock gate and discrete clock gating:
**Integrated clock gate                                      Discrete clock gate**
No clock skew btw latch and AND gate                   Ensure min skew btw latch and AND gate
Timing analysis & CTS handle ICG automatically      Specify latch clk pin as non stop pin for CTS
Setup & hold check modelled in library              specify the setup and hold time
Easy to use in flow                                 This adds complexity to the flow

**Enable Signal Timing**
--> Synthesis assumes that the clock signal arrives at all registers and clock gates at same time (within skew).
--> Clock signal reaches the clock gating cell earlier than it reaches the registers.
--> Timing constraints on the enable signals need to be adjusted.

The closer the ICG cell to the registers, the less constrained the enable signal. Pro: EN pin timing less critical. Con: clock tree length before clock gating is high.
If the ICG cell is close to port, then Pro: More clock tree buffers come after clock gating - so more power can be saved. Con: There might be tight timing constraints between ICG EN pin and registers as the skew is gonna be high in this kind of design.

**Impact of Clock Gate Fanout on Power and Timing**
**Large max fanout                                            Small max fanout**
Fewer ICG cells, more constrained EN pin timing     More ICG cells, easier to meet EN pin timing
Better power reduction                                    Power might be affected

**Impact of Clock Gate Fanout on CTS**
**Large max fanout                                    **Small max fanout**
Unbalanced clock structure                         More balanced clock structure
Depending on skew req, may need processing           Easier to meet CTS QoR
for CTS QoR

**Summary -**
Large max fanout --> results in best power savings and reasonable CTS QoR.
Small max fanout --> More balanced clk structure - easier to meet EN pin timing comparatively.

Experiments have shown that using a balanced fanout of 128 or 256 results in improved CTS QoR.

**Clock Gating Usage During placement Optimization**
