---
layout: post
title: 5 stage pipeline
---

RISC: Sequence of simple operations.
CISC: A few no. of complex instructions for a same task. Instructions of variable length and variable cycles to complete.

MIPS RISC architecture:
Pipeline doesn't reduce latency of single task, it improves throughput of entire workload.

- Instruction Fetch: Based on PC, fetch instruction from memory. Increment PC.
- Instruction Decode / Reg fetch: decode the instruction + register read operation. If a load word instruction, we will have opcode, R1, R2 and contents of R2.
- execute / Effective address calculation: If operation is memory reference: LW R1, 8(R2) or SW R1, 8(R2) . in both these cases: content of R2 is added to 8 i.e effective address calculation happens.
- Memory access: This stage is used only by load / store. If it is ALU operation, it is bypassed to MEM/WB register.
- Write back: For ALU or load instruction, WB is last step. Write back to register which is in btw IF/ID and ID/EX.

Control Hazard on branches
If branch is taken, and since some instructions are already in pipeline, in this case we have to flush out all instructions. Branch can be resolved in second stage itself instead of having to wait till Memory stage.

Flushing 3 instructions is more complex than flushing 1 instruction, so we will have BP in second stage. 4 branch hazard alternatives:
1. stall until branch direction is clear.
2. predit branch not taken.(and execute successor instructions in sequence). If branch is actually taken we have to squash instructions in pipeline.
3. predit branch taken. (but branch target address is not known by IF stage, only at end of decode stage we will know target address). MIPS still incurs 1 cycle branch penalty.
4. delayed approach. (identify a instruction anf put that after branch instruction which is to be executed for sure whether branch taken or not).

Branch prediction Buffer is attempted when branch instruction is fetched (IF stage or equivalent).
Branch Target Buffer
RAW - read after write hazard

##Tomasulo Algorithm
To allow out of order execution, split the ID stage into two:
- Issue (decode instructions, check for structural hazards, )
- read operands - wait until no data hazards, then read operands.

In dynamically scheduled pipeline, all instruction pass through issue stage in order; however they can be stalled or bypass each other in second stage and thus enter execution out of order.

For all stores, it will wait for all the address and data are properly received to write back.

Schedular and register status indicator allow for operand forwarding and register renaming in out of order execution.

Reasoning for dynamic scheduling: Rearrange execution order of instructions to reduce stalls while maintaining data flow. Trying to explore instruction level parallelism.

OOO execution introduces possibility of WAR and WAW hazards.
WAR: Add R2, R1, R0 and Sub R0 , R5 , r6
waw: Add R2, R1, R0 and sub R2, R5, R6

WAR and WAW addressed by register renaming.
Ex: DIV.D F0, F2, F4
ADD.D F6, F0, F8
SUB.D F8,F10, F14
MUL.D F6, F10, F8

## How register renaming is done explained Below
Based on above example, need not update F6 from ADD we can give a new name S. F6 is updated only from MUL.D.

Result values are available on common data bus from where operation forwarding can happen. Write result into CDB there by it reaches the reservation station, store buffer and register file with name of the execution unit that generated it.

Stores must wait until address and value are received.
