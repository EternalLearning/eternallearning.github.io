---
layout: post
title: Understanding various steps in cts
---

clock opt engine starts with an initialization step, which checks for pre-requisites and placement overlaps before building the clock tree network. The quality of the clock tree is highly dependent on the quality of input given to the tool. For example, a very tight transition may lead to high buffering, whereas a relaxed transition leads to a substantial violation at signoff. Hence, it is important to understand the flow inputs that are passed to the tool.

When Clock opt engine starts, it will echo all the non-default properties/attributes , we could check for this in log file. "Non-default CCopt properties:"

### Steps in clock opt engine
### 1) Clustering
During this step, Clock opt engine will build only a DRV-aware clock tree and will not balance the clocks. At the start of this step, CCOpt will print the maximum driver distance and the unit delay for the clock buffer/inverter from a user-provided list or from the library. The following portion of the log file indicates the same:

grep "Buffer max distance"

- Indicates the number of buffers (b), inverters (i), clock gates (icg), logic cells (l), and total cells used during clustering.
- Points to the remaining transition violation present in the design.
- The tool clearly echoes different types of transition target sets and the maximum transition achieved for those transition targets.
- Additionally, CCOpt runs also report the minimum, maximum, and average insertion delay for each skew group in the design.

### 2) Balancing
During this step, Clock opt engine will balance the design per the skew group constraint. Look for the pattern balancing in the log file. This pattern will repeat multiple times in the log file.

Look for the pattern “Clock DAG stats after approximately balancing Bottom fragment up” pattern to understand the design QOR change during balancing.

- Skew group information will point to min, max, avg ID, and actual skew vs. target skew.
- It will also report the percentage of sinks in that skew group meeting the skew target.

Comparing the cell stats after the clustering and balancing step can help understand the number of cells used for balancing the design. Usually, only the incremental number of cells is used to balance the clock skew. If this count is very high, it may point to the skew group setup issue.

### 3) Routing
During this step, Clock opt engine will route all the clock tree nets using a Nanoroute engine.

Look for the pattern “Clock DAG stats after routing clock trees” in the log file to see the design QOR after the clock route step.

### 4) Post-conditioning
This step is run to clean up any minor degradation after the clock routing.
get_ccopt_property -help *post_conditioning*

Post-conditioning has user controls to:
- Fix DRVs by sizing of cells and buffering.
- Fix skew by sizing of cells and buffering.
- Perform eco routing after these fixes.
