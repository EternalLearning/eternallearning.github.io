---
layout: post
title: Interface timing miscorrelation block vs top level
---

## IO Timing Miscorrelation between Block Level and Top-Level

There are a couple of reasons, which lead to miscorrelation between block-level and top level IO timing:

1. Inappropriate modelling of the driver pin and output load for the IO pins.
2. Missing constraints.
3. Different StarRc and signoff tool recipe or version used at the block level and top-level.
4. Mismatch in de-rating numbers in the block level and top-level runs.
5. cross talk on clocks
6. if budgetting is done using ideal clock based.
7. If uncertainity numbers are different in block and top level.


## From another article
In the hierarchical design flow, even after the timing closure is done at the block level, the designer might observe certain new violations inside the block when the block is plugged into the top. We shall cover the diff scenarios that might cause such a mismatch  and provide recommendations for each of these cases to be considered during block level STA such that mismatch between top and block is eliminated.

The following main scenarios will be discussed in subsequent sections:

1. SI differences due to aggressors’ timing window expansion at top, covered within the “Timing Window Common Path Pessimism Removal” section.

2. Path CPPR Extension.

3. Constraints modelling differences between the block and top, covered under the following two sections:
- Latency modelling and input delays.
- differences in clock definitions.

4. cycle-to-cycle SI variations in common clock path.


## Timing window common path pessimism removal
SI impact on a net depends on the relative switching times of the attacker and victim nets. The aggressor attacks the victim with its full strength if it switches at the same time as the victim, and the impact of the aggressor on the victim declines as the distance between switching times increases.

So, during SI analysis, the closest possible aggressor net to the victim switching time is found using the arrival timing window (TW) of the aggressor/victim net.

The computation of the closest switching time based on these arrival times can be pessimistic if both the aggressor and victim share the same clock path. This adds pessimism in the crosstalk analysis.

This pessimism is predominantly seen in hierarchical flows on the block-level registerto-register paths, when the block is instantiated in the top module due to the long toplevel clock path having different min/max delays.

**Recommendation**
Tool has the capability of removing this pessimism by adjusting the aggressor arrival window with the difference in min-max delay of top common clock path.

timing enable timing window pessimism removal has to be true to enable this.


## Path CPPR extension
**Recommendations:** Apple realistic min and max clock larencies (for both rise and fall transitions) for all clock ports at the block level. Tool will create timing window based on these latency values, and correspondingly consider path CPPR timing window for victims in PBA.

## Latch Modelling and Input Delays
At the block level, it is important to accurately model the min/max source latency for the clock coming from the block port, and the min and max input delay at the input ports of
the block.

It is known that at the top level, there will be a TW reaching the clock port of the block, and not just a single transition. Therefore, the absence of the different min and max clock source latency values in constraints at the block level will cause the TWs of aggressors to be narrower as compared to the ones where the block is plugged at the top.

This, in turn, will lead to optimistic slack values for many paths while doing block-level STA, because the full impact of aggressors will not have been considered.

**Recommendations**
- Different min and max input delays should be applied in the block-level constraints.
- The set_driving_cell command should be used on all input ports including the clock port with appropriate driving cells to avoid any slew mismatches when the block is plugged at the top.
- Add extra pessimism in the min/max latency values for both rise and fall.
- Apply different min and max clock source latency values at the block level for both rise and fall transitions.

## Differences in clock definitions
Some designers follow the practice of performing the timing closure of a block with just the fastest clock at the clock port, while there might be several clocks possible at the
clock port at the top level. This kind of approach might lead to optimistic timing at the block level, especially from the hold perspective.

**Recommendations**
- The recommendation here is to have all the relevant clocks in the block-level constraints as well catch such violations at the block level itself.
- Additionally, an appropriate relationship between these clocks should also be defined at the block level. For instance, in Figure 6, only one of the two clocks, clk1 and clk2, will be present inside the block at one point. Therefore, these clocks can be defined as physically exclusive in the block-level constraints.

## Cycle to Cycle SI variation in common clock path
Tool does not consider the SI delay of the common path between the launch and capture clock path for the CPPR adjustment for setup. For hold, the tool adjusts for both
base as well as SI delays of the common clock path.

The reason behind this behavior is that setup is checked with different clock edges reaching the launch and capture flops,
and SI delay can vary from cycle to cycle. On the other hand, hold is checked with the same clock edge for both launch and capture flops and therefore, the SI impact will be identical on both paths.

Now, when a block, with the timing closure done, is plugged onto the top level, there might be an additional logic on the clock path from the top clock source to the block clock port. This will mean an increase in the common clock path for all the launchcapture pairs inside the block being triggered by this clock. Any SI delay on this common clock path might cause the setup slacks to degrade inside the block timing
paths, and many paths that were otherwise clean at the block level might start violating.

**Recommendations**
- use set clock latency -jitter to model the top level SI impact on block as jitter at block level.

The jitter value specifies the portion of the source clock latency value that can have a cycle-to-cycle variation. When calculating the CPPR adjustment, this portion is removed
from the CPPR adjustment for various clock edge checks (for example, setup), but it remains as part of the CPPR adjustment for the same clock edge checks (for example, hold).
