---
layout: post
title: What affects the cell delay and interconnect delay
---

In this article we will discuss in details about the affects in cell delay calculation and interconnect delay calculation.

** Cell Delay** depends upon input transition and output fanout.
- Timing delay between input pin and output pin of a cell.
- cell delay information is contained in the library of a cell.

**Net Delay** Interconnect delay between driver pin and a load pin.

To calculate NET delay we would require below info.
- characteristics of driver cell.
-  load characteristics of receiver cell.
- RC value of the net.
