---
layout: post
title: Different Setup and Hold fix methods!
---

## Ways to fix Setup timing violation
**Setup** time is defined as the minimum amount of time before the clock's active edge by which the data must be stable for it to be latched correctly. Any violation in this required time causes incorrect data to be captured and is known as a setup violation.

![Flops](https://www.edn.com/wp-content/uploads/media-1168670-setupandhold1.jpg)

![Waveforms](https://www.edn.com/wp-content/uploads/media-1168671-setupandhold2.jpg)

The setup time requirement equation is as below:

Tc-q + Tcomb + Tsu <= Tclk + Tskew

Tc-q + Tcomb >= Thold + Tskew 
