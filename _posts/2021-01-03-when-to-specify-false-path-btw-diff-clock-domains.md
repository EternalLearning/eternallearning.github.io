---
layout: post
title: When to specify false path between diff clock domains?
---

We need to understand whether the clock domains are related or independent of each other. It depends on whether there are any data paths that start from one clock domain and end in the other clock domain. If there are no such paths, you can safely conclude that the two clock domains are independent of each other. This means that there is no timing path that starts from one clock domain and ends in the other clock domain.

If there are data paths that cross between clock domains, a decision has to be made as to whether the paths are real or not.

- An example of a real path is a flip-flop with a 2x speed clock driving into a flip-flop with a 1x speed clock.
- An example of a false path is when the designer has explicitly placed the clock synchronizer logic between the two clock domains. In such a case, even though there appears to be a timing path from one clock domain to the next, it is not a real timing path since the data is not constrained to propagate through the synchronizer logic in one clock cycle. Such a path is known as a false path because the clock synchronizer ensures that the data passes correctly from one domain to the next.


## This is a separate topic but including in this article
**How to set a false path between inter-clock paths through a mux**

![figure1](https://c.na14.content.force.com/servlet/servlet.ImageServer?id=015d000000BaiRu&oid=00Dd0000000c1Z9&lastMod=1481261504000)

It is required to avoid reporting violations when launch path and capture clock are between the following clocks:

ck1 and ck2

ck1 and ck4

ck3 and ck2

ck3 and ck4

**Solution**
set_false_path -through **will only apply to data paths and not to clock paths because it is a data path exception**, as are set_multicycle_path and set_max_delay. So, you cannot reference clock path elements (besides the clock waveform itself) in these constraints.

Instead, you can use the set_clock_exclusivity command to achieve this.

set_clock_exclusivity M1/Y
