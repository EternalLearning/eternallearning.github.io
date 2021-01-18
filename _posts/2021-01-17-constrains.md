---
layout: post
title: Constraints
---

In this article we shall discuss about the different constraints we give for a block.

1) Creating clocks and defining clock groups if necessary.

func clocks, test clocks, any generated clocks.
create_clock -name <> -period <> -waveform {} [get_pins ]
set_clock_groups -name " " -async -group

2) Timing exceptions like false paths or multi cycle paths.  

set false paths for cross clocks and multi cycle paths

3) group_path to give cost func

4) Port delays. Timing constraints applied on input and output ports.  
set_input_delay -clock [] -add_delay -max_delay [get_ports ]
we give both max delay and min delay
set output delay we give both max delay and min delay

set_driving_cell -lib_cell  <> -library <> -pin ZN [get_ports ]
set_load <> [get_ports ]

both set_driving_cell and set_load if not given the input and output ports assume infinite drive strength.

5) max cap, max fanout , set case analysis, max leakage power

6) set_ideal_network and ideal nets, set_dont_touch, set_dont_use true [get_lib_cells ]

7) set_clock_uncertainty -setup <> [get_clocks ], set_max_time_borrow <> [get_clocks ], set_clock_latency -clock_gate <> [get_pins ]
