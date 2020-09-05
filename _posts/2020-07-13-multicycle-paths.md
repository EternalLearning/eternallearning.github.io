---
layout: post
title: MultiCycle Paths!
---
If no multi-cycle is defined, then default setup check happens after one clock cycle and hold check happens at same clock edge as launch. This looks something like below:

![figure2](data:image/png;base64,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)

Incase, combinational datapath delay is such that it takes 3 clock cycles to propagate the data from launch to capture flop then there should be MCP defined for setup check:

set_multicycle_path 3 -setup -from [get_pins <launch_flop/Q] -to [get_pins <capture_flop/D]

The important note is that if we have not defined the corresponding hold MCP, then default hold check applies (i.e hold check will happen one cycle prior to setup check with MCP).

Below figure gives more details if observed carefully. The setup check is done at clock edge 3, while default hold check will be done at clock edge 2 (i.e clock edge prior to setup clock edge with MCP).

![figure1](data:image/png;base64,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)

There is a problem with such MCP constraint (where only setup MCP is given but not hold MCP). The intended hold check should be done at the same launch edge i.e o (launch) to o (capture). So, the corresponding hold constraint should be:

set_multicycle_path 2 -hold -from [get_pins <launch_flop>/Q] -to [get_pins <capture_flop>/D]

So, in general in most of the designs, a MCP of setup of 'N' clock cycles should be provided with corresponding hold MCP of '(N-1)' clock cycles.