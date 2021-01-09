---
layout: post
title: Timing Models
---

For full flat chip timing analysis we need to read in gate level netlist alogn with spef/sdf, timing libraries and constraints. Using this approach designers should wait till all blocks completion prior to performing full chip timing.

Hierarchical STA flow allows you to partition diff blocks using timing models which shld completely model the full input/output timing characteristics without requiring the complete netlist of block.

Internal reg2reg paths are usually discarded.

Hierarchical STA benefits:
- reduces runtime and memory usage.

## Types of Timing Model:
1. ETM Extracted Timing models
2. ILM Interface Logic Models.
3. QTM Quick Timing Model.

The two most common are the Extracted Timing Model
(ETM), which takes the form of a Liberty model (.lib), and the Interface Logic Model (ILM), which takes the form of a reduced netlist of interface logic and associated parasitic capacitance information.

**ETM-Extracted Timing Model**

ETM creates a timing arc for each path (port to port, port to register or register to port).

**Lets discuss some deficiencies using ETM**
1. Model generation, validation and merging.

ETM generation and validation needs a special flow setup.  In addition, ETMs that include multiple-constraint modes must be merged into a single ETM for usage at the top level of the chip.

2. SI-aware ETMs
Comprehension of SI in ETM generation is not very robust. Design teams typically adapt to the methodology  by making interface logic SI clean before extracting ETMs. Generation of ETMs with SI significantly adds to the model generation runtime because in-context aggressor nets need to be accounted for. The generation of timing window information at the top level of the design is also necessary.

3. MMMC ETM generation
ETMs can be merged across the modes but not across the corners.

**QTM Quick Timing Model**

In early stages of design cycle, if a block does not yet have a netlist, you can use a quick timing model to describe its initial timing. Later in the cycle, you can replace each quick timing model with a netlist block to obtain more accurate timing.

**ILM Interface Logic Model**

ILMs remove the register-to-register logic and preserve the rest of the interface logic in the model . The
components within an ILM include a netlist, parasitic loading, constraints, and aggressor information pertinent to the preserved logic inside the ILM. ILMs are highly accurate and can also speed up analysis considerably, while reducing the memory footprint.

1. Combinational logic from each input port to the first stage of sequential elements of block.
2. the combinational logic from last stage of sequential elements to each output port of the block.
3. the clock paths to these sequential elements.
4. combinational paths from input ports that do not encounter a sequential element and pass directly to an output port.

**Limitations using ILM**

ILM limitations include over-the-block routing, constraint mismatches, data management, arrival pessimism, latch-based designs, specific flows, and special stitching.

1. Over the block routing
If blocks are characterized without over the block routing in place, the timing could be optimistic since the SI impact of the over the block routes is not accounted for. This included the cross coupling from over the block aggressor nets to victim nets within the block.

2. Constraint mismatch
Additional bookkeeping is needed to preserve the context information. Mismatches in constraint are a significant headache for designer and can be a major setback for successful hier analysis.

3. Pessimism in arrivals due to timing window CPPR.
Timing windows are pre computed during ILM creation, and any CPPR effect from top level requires special handling to remove the pessimism in arrival times and windows.

4. Latch-based designs
In case of latch based design, increased storage needs for side capacitances and SI aggressors impact ILM creation and hier timing analysis runtime efficiency.

## Differences between ILM and ETM
Both ILM and ETM can be used in hier STA flow when flat analysis is not possible because of runtime and/or memory usage. An ILM offer more visibility into the netlist, which can result in easier verification, but provides less IP protection.

ETM is just like .lib means we have timing info till the pins not to the first level (i mean not to the first gate of FF) but if we want to do timing analysis between partition level FF to FF then we can do that with ETM.
