---
layout: post
title: Placement of Clock Gating Cells
---

### Should the clock gating cells be placed closer to clock source or closer to sink flops ?

Let us explore the pros and cons of two scenarios below:

1. **Clock Gating cells placed near the source:** As shown in below figure, placing clock gating cells near source increase the uncommon path in clock structure. We know that we need to talk OCV into effect because cells in uncommon path may behave different for PVT variations. So, this extra uncertainty or pessimism needs to be taken into account while doing STA analysis. Such a scenario is hostile to timing engineers.

However, from power saving prospective this scenario is quite favorable. As soon as the clock gate is "off" all the clock buffers in the path are switched off. So, there is no toggling and dynamic power is reduced (switching power dissipation is not possible).

![Figure1](http://3.bp.blogspot.com/-o-yNxIaMpKI/UeAJEvTYiTI/AAAAAAAAAeI/ELGWri2xdL0/s1600/Case_1.png)

2. **Clock Gating cells places near the sink:** As shown in figure below, placing the clock gating cells near to sink flop reduces the uncommon clock paths making the timing easy to meet. But, the dynamic power is not saved. Because even if clock gate is enable all the clock buffers before that still toggle.

![Figure2](http://1.bp.blogspot.com/-tqYP1d-ClqI/UeAGP55LDPI/AAAAAAAAAd0/zF02I9tYhAI/s1600/Case_1.png)


So, there is no one final answer for the clock gate placement. Depending on the design specs and requirements we can optimize for performance and/or power. 
