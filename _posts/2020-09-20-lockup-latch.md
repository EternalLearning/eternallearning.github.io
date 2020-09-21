---
layout: post
title: Lockup Latch
---

Why do we need lock up latch and how are we gonna benefit from lockup latch ?
A lock-up latch is nothing more than a transparent latch used intelligently in the places where clock skew is very large and meeting hold timing is a challenge due to large uncommon clock path. That is why, lockup latches are used to connect two flops in scan chain having excessive clock skews/uncommon clock paths as the probability of hold failure is high in such cases.

For instances, the launching and capturing flops may belong to two different domains (as shown in figure below). Functionally, they might not be interacting. Hence, the clock of these two domains will not be balanced and will have large uncommon path. But in scan-shift mode, these interact shifting the data in and out. Had there been no lockup latches, it would have been very difficult for STA engineer to close timing in a scan chain across domains.

![figure1](https://www.edn.com/wp-content/uploads/contenteetimes-images-edn-systems-design-design-fsenhtimclo-figure3.jpg)

Here we are assuming launch and capture flops to be rising edge triggered. Without lockup latch, it is a simple setup/hold check. if
launch and captured clock common points are far apart, there could be quite a large clock uncertainty. This is very typical for scan or test clocks where last flop in one scan chain is in a specific clock domain and first cell of next scan chain is in a different clock domain. There could be large hold violations for such paths.

Low phase latch, launches data at the falling edge of the clock and remains transparent during low phase. Essentially by introducing the lockup latch, we moved launch edge from rising to falling, and now our launch and capture edges are a clock phase apart, we have a clock phase worth of margin(slack) to meet the hold time requirement.
