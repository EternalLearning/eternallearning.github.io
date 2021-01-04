---
layout: post
title: How to increase clock latency source window and ensure block level timing closure in top level context?
---

This is a follow up article based on previously written io timing miscorrelation article.

In hierarchical designs, I can see reg2reg timing violations inside the blocks which were timing met during the block-level STA.

This is due to the presence of the external clock latency that increases the overall timing windows used in the top-level flat analysis, so the SI contribution calculated within the block in the top-level context is increased compared to what was used to sign off the block standalone.

**Recommendations**
One possible solution to avoid top-level violations not seen at the block-level STA is to use infinite timing windows for SI analysis:

**si use infinite timing window true**

But in most cases, this would be a too pessimistic approach that will increase area/power. It may also not be feasible for very tight blocks. So, the alternative would be to account for the external clock variation by adding an early/late arrival time on the clock ports.


![figure1](https://cadence.my.salesforce.com/servlet/servlet.ImageServer?id=0150V00000DJyYjQAL&oid=00Dd0000000c1Z9EAI)

increase_clocks_source_latency_window     latency_window   {jitter_window}
