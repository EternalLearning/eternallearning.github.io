---
layout: post
title: Various issues at lec
---

## How to model sequential constant registers in lec?
By default tool does not model sequential constant registers.

There are several commands to enable modeling of constant registers to logic 1 or 0, perform constant propagation to its downstream logic, and remodel the netlist accordingly.

set flatten model -seq_constant or

**analyze setup can automatically resolve the setup issues related to constant optimization, gated clock and phase mapping.**

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

## From another article
**Impact of RTL design for ease of verification**
Below are the factors of RTL design that can affect the ease of verification:
- Don't care conditions in RTL.
- Structural implementation.
- Partitioning of designs.

Coding RTL with previously mentioned factors in mind to help avoid aborts in verification.

***Remove Don't care conditions in RTL***
Dont' care conditions are created in RTL by
- Array index out of range reads.
- Paralle/Full case statements.
- X assignments.
- Range constraints.

A dont care condition in RTL can be synthesized to any boolean function.

***Use structural descriptions instead of behavioral descriptions***
For complex arithmetic expressions, introduce intermediate signals and additional assignments.

***Design Partitioning in RTL***
Partitioning design breaks into smaller pieces for ease of verification.

**Synthesis for ease of verification**
Try to do RTL to mapped first and then mapped to optimized netlist fev.

**What is an abort?**
Abort is a notion that LEC finds it not likely to prove or disprove the equivalence relationship withing a reasonable amount of time. Abort also means that LEC will need much more effort and time on the proof after which it still may not resolve them.

**What causes abort?**
- Design size. The larger the size of the design, the more difficult it can be to compare.
- Design similarity. Advanced synthesis optimization usually decreases the design similarity.
- Design function. Synthesis has more chance to optimize design with datapath or dont care conditions.

**LEC methods to resolve abort**
- Hierarchical compare.
- Multi threading.
- Compare engine.
- Structural partition.

**Advanced LEC techniques to resolve abort**
- analyze abort -compare

## From another article
**Why is an extra round of analyze setup needed even when there is a set analyze option -auto in the do file?**
There might be modelling issues sometimes. Generally, only one round of analyze setup in addition to set analyze option -auto shld be able to resolve modelling issues. Also, running the analyze setup command after compare command increases the probability of resolving modelling issues because tool has better understanding of design after comparison.

## My FEV experiences
I did some add renaming rules and multidimensional to single dimensional ports map and scan constraints issue.

For renaming rules:
Q) Why we see renaming rules problem:

 I am doing a Conformal LEC comparison where the golden design has individual flops and the revised netlist has multi-bit flop libcells. While trying to do a comparison in LEC, I am getting several non-equivalences related to these multi-bit flops. It seems LEC is not able to map the multi-bit flops correctly.

## From another article - This is very nice and imp
DFT Structures like scan chains, compression, and MBIST are inserted in the design at various steps during the synthesis and the Place & Route flows. Since DFT logic does usually not present in the golden/reference design, equivalence checking presents
some challenges to exclude the newly inserted logic from the comparison process.

Correct constraints save the debug and run-time. When a design is under-constrained, nonequivalences will be encountered, but over constraining can mask potential bugs.

It assumed that the equivalence checking is performed at three different stages of the design flow.
a) RTL vs intermediate netlist: post initial mapping but before incremental synthesis/compilation or scan chain connection.
b) intermediate vs final syn netlist.
c) final syn netlist vs signoff netlist.

In some cases using diff syn tool, step a and b are merged and only 2 verification steps are done.

Use, write_do_lec as a starting point for DFT constraining. However, it is users responsibility to validate the constraints and add or remove any if needed. Syn tool uses very conservative approach when adding DFT constraints in generated dofile as preventing any false equivalent results.

**RTL vs intermediate netlist**
Disabling scan (both fullscan and compression) means disabling both test mode and scan enable signals.

add pin constraints <0/1> <scan_enable/test_mode> -both

Internally generated scan enables shld be constrainted away too, by adding primary input on Q pins of flops and tehn placing a constraint on the net.

add primary input </Q> -pin -both
add pin constraint <0/1> </Q> -both

If a new primary inputs is created during synthesis for scan chains, or for any DFT signals, we need to ignore those to run LEC.
add ignored inputs <newly created inputs> -all -revised
add ignored outputs <newly created outputs> -all -revised.

At the intermediate netlist most DFT structures are already inserted, so we need to constrain them away as extra logic exists in revised side but not in golden side.

add pin constraints <0/1> -revised

Disabling IEEE 1500 logic:
This is observed in my designs. IEEE 1500 wrapper logic has many diff insertion flows.

add pin constraints <0/1> <INTEST_SIGNAL> -revised
add pin constraints <0/1> <EXTTEST_SIGNAL> -revised

Depending on setup, if intest and extest signals aren't top level pins, you will need to cut a net and add a new pseudo inputs before constraining them.

add primary input <INTEST_SIGNAL> -pin -revised
add primary output <EXTEST_SIGNAL> -pin -Revised

**Between synthesis and final PR**, since no new DFT constructs are inserted, there is no need to constrain out any logic. The only exception to this is if scan reordering has been done, in which case it is mandatory to constrain away the SE signal so that the scan shift path is constrained away.

add pin constraints <0/1> <scan_enable> -both

**Hierarchical DFT flows**
In a hierarchical bottom-up flow, whether it is .lib/ILM or even black box based, we have to make some extra considerations to run LEC at the top level.

**Lockup latches**
Lockup latches should not be visible in LEC for intermediate vs. final comparisons, because in this comparison the scan path is not visible to LEC. However, there are some corner cases that a lockup latch might get mapped and not flagged as an unreachable point, resulting in NEQs. In those cases, it is important to understand what prevented those lockup elements from being flagged as unreachable. For example, let’s look at the case of a register, followed by a lockup latch, whose scan path is connected to an SI pin of an IP (black box for LEC). In this scenario, the flop can be scan reordered, while the lockup latch might not unreachable, for instance it has set or reset. There are other unlikely
scenarios that can legitimately cause a lockup latch to be mapped and compared, for example if it’s directly reachable from the compressor. In these cases, and once the problem is understood and it is safe to ignore this NEQ, we can delete these mapped points from comparison (delete compared point).
