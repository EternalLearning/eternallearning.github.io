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

12) How a latch gets inferred in RTL design?

A latch gets inferred in the RTL design:-

When there is no “else / default” statement in the “if / case” statements; in short if all possibilities of the conditions are not covered.
When all the outputs reg are not assigned the values in every condition of the “if / case” statement and some are left out, on the left out signals a latch gets inferred.

13) Does a latch get inferred when there is no else statement but multiple ifs covering whole functionality?

Conceptually no latch should be inferred but sometimes the synthesis tools are not intelligent enough and they might infer a latch. In order to avoid that, the safest way is to use an “else / default” statement in “if / case” respectively.

14) If there is an asynchronous feedback loop what is the problem?

If there is an asynchronous loop in the design the circuit becomes oscillatory or it may reach a stable state where it might get hung and it could not get out.

15) If an oscillatory circuit is there; what happens during (a) RTL Synthesis (b) Simulation?

(a) During the RTL synthesis, the synthesis tool will give a warning during synthesis about the combinatorial feedback loop.

(b) During the simulation the simulation will get stopped saying the Iteration limit reached.


16) Where can we use Linting tools ? Can we use them to debug syntax?

Linting tools are used to evaluate the design for the synthesizability of the design. These tools are use to check for potential mismatches between simulation and synthesis. No they are not used to check the syntax.

17) If no parameters in the always sensitivity list, how the always block executes?

It will repeat itself like a forever loop but the performance will degrade.

18) Tell the scenarios where synthesis error occurs.

A synthesis error can occur in the following scenarios:

- When there is multiple assignments on the same signal in two different blocks, “a multiple driver found” message will come.
- When there is mixture of asynchronous reset with some other signal and that signal is not used in the sensitivity list; basically mixing of multiple edges and synchronous and asynchronous elements are not allowed.
- No element in the always block sensitivity list.
- Mixing of B.A and N.B.A on the same signal in two different conditional statements.
- If reg datatypes are used in assign statements, etc.
