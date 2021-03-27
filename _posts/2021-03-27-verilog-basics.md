---
layout: post
title: Verilog basics
---

1) verilog code to swap contents of 2 registers with and without temporary registers.
with temp reg:

always @(posedge clk)
begin
temp = b;
b= a ;
a =temp;
end

without temp registers:

always @(posedge clk)
begin
a <= b ;
b <= a ;
end

2) Diff btw blocking and non blocking

The two are distinguished by the = and <= assignment operators. The blocking assignment statement (= operator) acts much like in traditional programming languages. The whole statement is done before control passes on to the next statement. The non-blocking (<= operator) evaluates all the right-hand sides for the current time unit and assigns the left-hand sides at the end of the time unit.

3) Variable and signal which will be Updated first?

Signals

4) What is sensitivity list?

The sensitivity list indicates that when a change occurs to any one of elements in the list change, begin…end statement inside that always block will get executed.

5)  In a pure combinational circuit is it necessary to mention all the inputs in sensitivity list? if yes, why?

Yes in a pure combinational circuit is it necessary to mention all the inputs in sensitivity disk other wise it will result in pre and post synthesis mismatch.

6) Write a Verilog code for synchronous and asynchronous reset?

synchronous reset means clock dependant so reset must not be present in sensitivity list:

always @ (posedge clk)

asynchronous reset means clock independent so reset must be present in sensitivity list:
always @(posedge clk or posedge reset)

7) What happens to the logic after synthesis, that is driving an unconnected output port that is left open (, that is, noconnect) during its module instantiation?

An unconnected output port in simulation will drive a value, but this value does not propagate to any other logic. In synthesis, the cone of any combinatorial logic that drives the unconnected output will get optimized away during boundary optimisation, that is, optimization by synthesis tools across hierarchical boundaries.

8) What is the implication of a combinatorial feedback loops in design testability?

The presence of feedback loops should be avoided at any stage of the design, by periodically checking for it, using the lint or synthesis tools. The presence of the feedback loop causes races and hazards in the design, and 104 RTL Design
leads to unpredictable logic behavior. Since the loops are delay-dependent, they cannot be tested with any ATPG algorithm. Hence, combinatorial loops should be avoided in the logic.

9) What are the various methods to contain power during RTL coding?

Any switching activity in a CMOS circuit creates a momentary current flow from VDD to GND during logic transition, when both N and P type transistors are ON, and, hence, increases power consumption.
The most common storage element in the designs being the synchronous FF, its output can change whenever its data input toggles, and the clock triggers. Hence, if these two elements can be asserted in a controlled fashion, so that the data is presented to the D input of the FF only when required, and the clock is also triggered only when required, then it will reduce the switching activity, and, automatically the power.

The following bullets summarize a few mechanisms to reduce the power consumption:

- Reduce switching of the data input to the Flip-Flops.
- Reduce the clock switching of the Flip-Flops.
- Have area reduction techniques within the chip, since the number of gates/Flip-Flops that toggle can be reduced.

10) What logic is inferred when there are multiple assign statements targeting the same wire?

It is illegal to specify multiple assign statements to the same wire in a synthesizable code that will become an output port of the module. The synthesis tools give a syntax error that a net is being driven by more than one source.
However, it is legal to drive a three-state wire by multiple assign statements.

11) What value is inferred when multiple procedural assignments made to the same reg variable in an always block?

When there are multiple nonblocking assignments made to the same reg variable in a sequential always block, then the last assignment is picked up for logic synthesis. For example

always @ (posedge clk) begin
out <= in1^in2;
out <= in1 & in2;
out <= in1|in2;
