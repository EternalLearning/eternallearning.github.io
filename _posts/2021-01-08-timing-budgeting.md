---
layout: post
title: Timing Budgeting
---

In hierarchical design flows, chip-level timing constraints must be mapped correctly to corresponding block-level constraints.

setup/hold time of flop will be part of output delays.
Input transition ans output load

Distribute the negative slack across all blocks equally.

Ways to handle latency for IO ports
- calculate average latency and apply that to virtual clocks. (used in implementation flow)
- know the clock parameters- latency, skew and adjust them in IO delays. (io budgeting flow supports this)

Second approach might help reduce differences btw block sta vs cluster sta .

Plan to use constraints from iob flow in block STA though block implementation is done using avg latency update.

From full chip, we calculate delay percentage and allocate the slack.

If ideal clock propagated budgets then 10% of clock period is used as skew in IO delays.

Budgeting clock latency in clock propagated mode. Tool includes clock latency in the constraints generated for clocks in propagated mode. The clock latency is included in set_input_delay and set_output_delay constraints. Clock latency is added to set_input_delay and subtracted from set_output_delay.
