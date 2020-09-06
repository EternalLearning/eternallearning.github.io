---
layout: post
title: ICG Methodology for power and timing QoR
---

**Benefits of Clock Gating:**

1) Dynamic Power savings

2) Eliminating multiplexers saves area. In earlier designs we used to use multiplexer for each register which consumes lot of area if we have to implement multiplexer for every register.

3) Easy to implement - no RTL code change is required. Clock gating is automatically inserted by the tool. Technology independent.

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

*--> Large or unlimited fanout*

- By default, no group bounds are created for the clock gate and its fanout during placement.

     a) avoid congestion around the clock gate

     b) you will get better overall timing QoR bcz placement of registers is based on timing,  not constrained by location of clock gate.

*--> Small fanout*

- To keep the clock gate and its register fanout together during placement, use:

      set physopt_disable_auto_bound_for_gated_clock false

      a) helps meet timing of the EN pin.

*When do we merge ICG's ?*

*ICG insertion is done during RTL synthesis, and let us say we are receiving the IP netlist and we integrate that then we can do merge_clock_gates to improve on power by optimizing/minimizing the ICG's in the design.*

**What is replicate clock gate ?**

Let us consider a scenrario, where we have ICG with 108 fanout, that is divided into approx 25 each fanout 4 ICG's instead of one. THis is called replicating the clock gate. Also, add buffers to drive registers that are not gated.

**What does replicate clock gate in ICC do?**

--> Replicates ICG with new instances using the same ref cell.
--> Balances the fanout of ICG based on design rule constraints.
--> Considers the location of registers.
--> Inserts buffers to drive registers that are not gated.
--> The number of clock gates increases
    a) ICG are larger than clock buffers and consume more power.
    b) Impact on power and area.

**When to replicate clock gates?**

Replicate ICG cells only when needed. If the target skew is not met and design has unbalanced clock structure then think about replicating ICG cells. This is done after place and before CTS.

**Prerequisites for replicating ICG cells**

1. Ensure that you have logically equivalent cells in the ref library. This allows the sizing of ICGs.
2. DRC constraints are set using set_clock_tree_options command.
3. To enable insertion of buffers to drive registers that are not gated, set cts_push_down_buffer true.
4. If you want to prevent the tool from using certain ICG cells, set dont_use on the cells.
5. use split_clock_net -objects -gate_sizing -gate_relocation command.

**Recommendations for RTL Synthesis**

--> Select the max fanout based on your design priority.
    a) Large fanout gives you more power savings
    b) Balanced fanout gives good CTS QoR
--> Use ICG cell (integrated latch based clock gating cells)

**Recommendations for Physical Synthesis/CTS**

1) Physical synthesis
   a) Use group bounds only when max fanout is small.
2) Clock tree Synthesis
   a) Replicate clock gates only if necessary
   b) Use DRC constraints to control the number of replicated clock gates.

**Scan testing on ICG** - So, to picturize the ICG cell we currently have is , EN signal coming from EN logic to D of latch. the clk of latch is negative sensitive and the output of latch Q is going to AND with clk signal to create ENCLK.

So, in the above case, we wont be able to test EN logic, latch, AND, and the sink registers from ICG. This logic is not observed.

To make them observable, we add a scan enable signal which gets ORed with EN logic signal.  
