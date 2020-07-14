---
layout: post
title: Unconstrained Paths
---

**Unconstrained paths** are paths without any timing constraints specified to them, i.e. set_input_delay, create_clock, etc.

Types of Unconstrained paths:

1) clock pin of flip flop is tied off

2) no input delay defined on input ports

3) no output delay defined on output ports

4) false paths


We can use **check_timing** to report all unconstrained paths in the design. 
