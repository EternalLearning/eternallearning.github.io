---
layout: post
title: Constraint management for source snychronous designs
---

A source-synchronous interface outputs the clock in addition to the data constrained with it. This clock port is used to sample data at the receiver that connects to the interface. There are different categories of source-synchronous interfaces.

For output source synchronous interfaces:

- The output port must have output delays correctly defined for all the clocks using the -add_delay option.
- Use the -combinational switch with create_generated_clock to prevent unwanted routing of clock paths through sequential devices.
- Take advantage of the set_clock_groups -logically_exclusive
constraint to set timing false paths between forbidden clocks, but still continue to count all clocks for noise crosstalk analysis.
- Use the set_clock_groups -asynchronous switch in case asynchronous clocks also merge into the clock mux.
- Configure the minimum pulse requirement for all clocks at the clock output port using the set_min_pulse_width constraint.

## understanding diff clock group Constraints
**Physically Exclusive**
- Defines clock group with clocks that cannot exist in the design at the same time. For timing analysis, the software considers these clock groups as false paths.
- The software does not consider aggressor interactions between clock groups that are physically exclusive for signal integrity considerations.
- From the perspective of SI analysis, all attackers that are physically exclusive with the victim net are ignored for both glitch and noise-on-delay analysis.

**Logically Exclusive**
- Defines clock group with clocks that do not have any functional paths between them, but coexist in the design at the same time. These are false paths for timing analysis.
- The software considers aggressor interactions between these clocks for signal integrity considerations.
- The -logically_exclusive parameter does not have any impact on SI analysis.

**Asynchronous**
- Defines clock group with asynchronous clocks. Asynchronous clocks have no specified phase relationship. These are false paths for timing analysis.
- Use the -allow_paths parameter to trace these paths in timing analysis.
- The software considers asynchronous nets to have infinite timing window overlap for signal integrity considerations.
