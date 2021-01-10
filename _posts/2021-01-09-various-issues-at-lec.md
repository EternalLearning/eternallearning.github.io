---
layout: post
title: Various issues at lec
---

## How to model sequential constant registers in lec?
By default tool does not model sequential constant registers.

There are several commands to enable modeling of constant registers to logic 1 or 0, perform constant propagation to its downstream logic, and remodel the netlist accordingly.

set flatten model -seq_constant or

analyze setup can automatically resolve the setup issues related to constant optimization, gated clock and phase mapping.

Using analyze setup :

set flatten model -seq_constant
set system mode lec
analyze setup -verbose
add compare point -all
compare

If logic cone leading to the constant register is very complex, tool might not be able to model the target registers to constant values all at once. Even after following one of the above 2 methods, if you are still left with not-mapped constant registers, use one of the following approaches to resolve the remaining constant registers:

**approach 1:**
If analyze setup is not able to resolve all the setup issues, including modeling of constant registers, enable another round of modeling after the analyze nonequivalent command  with either remodel or analyze setup.

analyze setup -verbose
add compare points -all
comparedanalyze noneq -verbose
analyze setup -verbose
add compared points -all
compare

**sequential constant group**
For a series of registers with the final register feeding back to the first register in the series (as shown in the following image), with both the asynchronous set and reset pins tied to inactive values, the synthesis tool optimizes all these registers to a constant value.

![figure1](https://c.na14.visual.force.com/servlet/servlet.ImageServer?id=015d000000AhanqAAB&oid=00Dd0000000c1Z9EAI)

There are two ways to correctly model the ‘sequential constant group’ of the registers in tool:

1. adavanced automatic Modelling

set analyze option -auto

2. Manual remodelling of all registers in the group
remodel -seq_constant_group <list of registers>

## How to model dont care flops in lec
![figure2](https://c.na14.visual.force.com/servlet/servlet.ImageServer?id=015d000000Ahl8zAAB&oid=00Dd0000000c1Z9EAI)

Synthesis tool has multiple options to optimize such registers in one of the following ways:

1. Leave the register as it
For this case, we will not need to set any additional option in conforma.. Tool will keep these as is to match with synthesis behaviour.
2. Optimize all such registrs to constant 0
3. Optimize all such registers to constant 1
For these 2 cases, we can use
set flatten model -seq_constant_x_to 0|1
4. optimize some registers to constant 1 and other to 0, depending on design optimization benefits.
We can tackle this in either of the 2 ways mentioned below.

- synthesis setup infomration will help tool in dealing with sequential constants. A log file contains information regarding the value that the sequential constant X register has been optimized to. Importing these files will help model these in golden design.
read setup information <filename> -type <rclog|vsdc|conformal>
- second method is, if such registers are small number, we can manually map these.
add instance constraint 0|1 <instant pathname>

## What are abort key points and how to resolve them
Abort points are key points that have not been proven equivalent or non-equivalent based upon the current compare effort algorithms. Abort points can be attributed to the following:

- Big logic cone sizes with large number of support points.
- Different structures with don't care conditions in a RTL to netlist comparison.
- Aggressive optimization during synthesis.

Ways to resolve aborts:
- increase the compare effort.
- use hier compare.
- analyze abort -verbose
- resolve non equiv key points first, if there is any setup issue or missing constraint causing non equivalence, resolving it may help resolve abort key points as well.
