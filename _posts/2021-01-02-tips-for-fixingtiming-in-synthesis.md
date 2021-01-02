---
layout: post
title: How to fix timing in synthesis
---

We will discuss various ways to fix timing in synthesis.

## 1. Validating timing Constraints

In most cases, timing violations are due to unrealistic I/O constraints, or from paths that should have been defined as false paths or multi-cycle paths. At the minimum, the user needs to run this command after reading in the SDC file.

report_timing -lint

This command will check for timing loops, missing I/O constraints (input_delay, output_delay), or registers that don’t have clock signal at the clock pins. When a timing loop is detected, RC will insert a loop_breaker_cell to break the loop.

How to spot unrealistic I/O constraints? A common practice is to review the input/output delays, input drive strength, and output loading to see if they are realistic. For example, if an external delay (input or output delay) takes more than 50% of the cycle time, it might be a good idea to review the external path to ensure the validity of the constraint.

## 2. Analyze violating Paths

Before making the attempt to resolve the timing violations, we need to understand the violating paths and perhaps to identify the cause of the violation. A useful command to get an overall result of the design is ‘report qor -levels_of_logic’. The option ‘-levels_of_logic’ shows the levels of logic in the critical
path.

If the critical path comes from a primary input or to a primary output, review the I/O constraints as discussed in the previous section. For the critical paths between registers, check for the following:

- Different launch/capture clock: if the launch clock and capture clock are different, it’s possible that the path might be a multi-cycle path or a false path.
- Datapath components: if there are datapath components (denoted by instance name with prefixes add_, mul_), use the ‘report datapath’ command to check the speed of the datapath components.
They should be implemented with ‘very fast’ structure on a critical path.

## 3. Strategies for fixing violations
Assuming the timing constraints have been reviewed and all constraints are valid, the following suggested strategies can be used to fix timing violations. They are not necessary in a priority order and can be used in combination.

**3.1 Creating different cost groups for I/O paths**
By default, the I/O paths (inputs to registers, registers to outputs, and input to outputs) are lumped in the same cost group with register-register paths if they are in the same clock domain. If the critical path (WNS path) is in the I/O paths (might be due to unrealistic I/O constraints), the register-register paths won’t be optimized (unless the endpoint_opto option is used) because they’re not considered as critical
paths of the cost group. Setting the I/O paths and register-register paths in a separate cost group let RC to optimize the critical path in the register-register path cost group separately from the I/O paths.

**3.2 Using multiple incremental synthesis**
It’s possible that RC can improve the result in the 2nd or 3rd incremental synthesis. After the 1st synthesis run, try high effort incremental synthesis to see if the violations can be resolved:

synthesize -to_map -effort high

**3.3 Using path adjust**
Tightening the constraint on a selective path would make the path become more critical and force RC to work harder on it. This trick can help closing timing for a small number of violating paths. The constraint needs to be set before mapping:

path_adjust -delay <delay> -from <start_point> -to <end_point>

Use a negative delay number to tighten the constraint (the effect of subtracting X delay from the normal cycle time). In contrary, a positive delay number would relax the constraint of the path, which can be used for area saving on non-critical paths.


**3.4 Changing datapath structure**
If the critical path has datapath components (adder, multiplier), use the ‘report datapath’ command to see the structure of the datapath components after mapping. RC starts with the very_fast
implementation for all datapath components after elaboration, then down grade them as needed during optimization. The datapath components on the critical path should be the very_fast implementation, if this is not the case, it might be tool issue. A workaround for this is to manually set the speed grade for
the datapath component before mapping, using this attribute:

set_attribute user_speed_grade very_fast <datapath_subdesign>

**3.5 Idealize high fanout nets**
RC will automatically idealize clock nets, asynchronous reset nets (only net connected directly to the asynchronous reset pin of a flop), and test signals (scan enable, test mode). For all other high fanout nets (HFN), it will try to buffer to satisfy meet timing or DRC rules. If the critical path has a buffer chain
due to HFN, idealize the net in RC and buffer HFN later during Place and Route for a more efficient buffer tree.

**3.6 Setting initial target**
At the beginning of the global mapping step, RC will estimate a target slack for each cost group. This estimated target is based on the libraries, the logic structure, and the constraints. RC will work toward this target number during the optimization process. In the logfile, search for the keyword ‘target slack’,
they’ll be printed before and after the global mapping step. A cost group with large negative target slack would normally indicate a problem area. If the constraints are clean, one could try to set the initial target to 0 or a positive number using the initial_target attribute. This will make RC to work harder on
this cost group.
