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
