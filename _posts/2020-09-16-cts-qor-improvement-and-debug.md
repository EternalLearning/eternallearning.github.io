---
layout: post
title: CTS quality enhancement and debugging
---

General guidelines to improve the cts quality are:
- Use buffers and inverters with a minimal difference between its rise and fall delay.
- Use default values for CTS constraints.

Tightening max_trans, max_cap, max_fanout will degrade the clock tree power considerably.
- Avoid setting the fixed attribute on clock tree cells. The fixed attribute can cause CTS to take non-optimal path from insertion delay point of view. Only specify on few cells if necessary.
- Tightening the max_buffer_levels setting can result in huge DRC violations and poor CTS results.

### Common setup for cts
- Give ref buf list.
- Give ref inv list.
- Create NDR rule with layers and shield and spacing.
- Give max trans.
- Give target latency.
- Give target skew.
- Primary delay corner.


### Debugging techniques

1)  Pre-CTS debug:
- check_clock_tree --> This command checks clock tree structure, constraints and clock tree exceptions.

2) Post-CTS debug:
from logs, clock tree debugger. Identify longest path delay, shortest path delay and maximum skew flops.

**Check in my design - local skew reported by report_clock_timing -type skew is larger than global skew reported by report_clock_qor -type summary because global skew takes derates into account unlike local skew.**
