---
layout: post
title: Power Reduction Methods
---

As power becomes increasingly significant in the advanced technologies more power reduction design methods are explored. There are several different RTL and gate level design strategies for reducing power. Clock gating is one of them and the most popular one. Other such is dynamic voltage and frequency scaling, but this is not very popular as it is difficult to design and implement.

Here we will discuss many ways to reduce power consumption.

1) **Supply Voltage Reduction -** The most basic way to reduce power is to reduce power supply voltage. Supply voltage in this generation technology node has fallen down to <1V. Lowering the supply voltage reduces the power consumption but also lowers the speed.

2) **Clock Gating -** is a dynamic power reduction method in which clock signals are stopped for selected flops/latches in the design when stored logic values are not changing.

3) **Multi-Vt Lib Cells -** To implement the same logic function sometimes we get different threshold voltage cells available in the library. A low-Vt cell has higher speed, but higher sub-threshold leakage current. A high-Vt cell has
low leakage current, but less speed. The synthesis tool can choose the appropriate type of cell to use based on the tradeoff between speed and power.

***Need to write why leakage increases with low vt***

4) **Multi Voltage Design -** - Different parts of the chip can have different voltage domains. Having multiple power domains on the same chip introduces more complexity.

**Level Shifter -** Where a logic signal leaves one power domain and enters another, if the voltages are significantly different, a level-shifter cell is necessary to generate a signal with the proper voltage swing. A level shifter cell itself requires two power supplies that match the input and output supply voltages.

***Need to write whether we need level shifer from both low to high and high to low voltage domains or only one***

**Power Switching -** Power switching is a power-saving technique in which portions of the chip are shut down completely during periods of inactivity. Power switching has the potential to reduce overall power consumption substantially
because it lowers leakage power as well as switching power. It also introduces some additional challenges, including the need for a power controller, a power-switching network, isolation cells, and retention registers.

A power controller is a logic block that determines when to power down and power up a specific block.
