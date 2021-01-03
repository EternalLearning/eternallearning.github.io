---
layout: post
title: Understanding check_timing
---

**clock_expected**
This warning indicates a missing signal of type clock on a clock pin
ofterm termed as clock phase. If there is any active, ie, non-disabled sequential arc from or to the clock pin & clock pin does not get clock signal, then this warning is flagged.

**clock_not_propagated**
This indicates that ideal clock is reaching the clock pin. In this
example, ideal clock is reaching to dff2/CK pin since the set_propagated_clock command does not cover ck2.

**clock_gating_no_data**
This indicates that no data signal is reaching the clock gating
enable pin (A1/A) due to the disabled timing arc between A and Y pin of instance B1.

**uncons_endpoint**
This indicates that no constrained signal is reaching the end point.
No clock is propagated beyond the pin C1/Y, because of the set_clock_sense constraint. Hence, no clock phase is reaching the clock pin of dff1 flop; given this, no reference timing is defined for dff1/D. For primary output port out, there is no reference
signal launching dff1/Q to the the out port. Furthermore no set_output_delay is defined at port out.


**no_drive**
This indicates that no drive constraint is specified on the specific port. In this example, drive constraint (either set_drive or set_driving_cell) is missing on ck port in SDC.

**no_input_delay**
This indicates that there is no input delay assertion with or without
the -clock option specified. In this example, no input delay has been specified for in1 port in SDC.

**Loop**
A combinatorial feedback loop is formed when the output of either a gate or a combinatorial path is fed back as an input to the same gate or to another gate earlier in the combinatorial path without any sequential element inbetween. In this case, the combinational loop starts and ends at pin A1/Y.

Since STA engines cannot time combinational loops, they need to get broken.

**data_check_multiple_reference_signal**
This indicates that multiple clocked signals are reaching the data check reference pin. In this example, set_input_delay on the in1
port is defined with respect to clocks ck and ck1 and hence, multiple clocked signals are reaching at the O1/A pin.

**multiple_clock**
This indicates multiple clocks are reaching the clock reference pin. In this example, with ck1 and ck two clocks are reaching to dff1/CK pin. As this can not happen physically, the user is responsible for making sure that only a single clock is propagating to the clock pin of a sequential element.

**constant clock pin**
This indicates a constant applied in the fanin of the reference
clock pin is reaching the clock. In this example, the constant applied using the set_case_analysis command on O1/A is reaching dff1/CK and avoids the clock ck being propagated through the OR gate.
