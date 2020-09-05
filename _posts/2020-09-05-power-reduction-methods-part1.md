---
layout: post
title: Power Reduction Methods - Part1
---

As power becomes increasingly significant in the advanced technologies more power reduction design methods are explored. There are several different RTL and gate level design strategies for reducing power. Clock gating is one of them and the most popular one. Other such is dynamic voltage and frequency scaling, but this is not very popular as it is difficult to design and implement.

Here we will discuss many ways to reduce power consumption.

1) **Supply Voltage Reduction -** The most basic way to reduce power is to reduce power supply voltage. Supply voltage in this generation technology node has fallen down to <1V. Lowering the supply voltage reduces the power consumption but also lowers the speed of the device. In addition, the transistor threshold voltage must be lowered, causing more problems with noise immunity, crowbar currents and sub-threshold leakage.

The lower voltage swing makes it more difficult to interface the chip with external devices that require larger voltage swings.

2) **Clock Gating -** is a dynamic power reduction method in which clock signals are stopped for selected flops/latches in the design when stored logic values are not changing. The main challenges of clock gating are finding the best places to use it and creating the logic to shut off and turn on the clock at the proper times.

3) **Multi-Vt Lib Cells -** To implement the same logic function sometimes we get different threshold voltage cells available in the library. A low-Vt cell has higher speed, but higher sub-threshold leakage current. A high-Vt cell has low leakage current, but less speed. The synthesis tool can choose the appropriate type of cell to use based on the tradeoff between speed and power.

For example, use low vt cells in timing critical paths for speed and high vt cells everywhere else for lower leakage power.

*Explanation why low-vt cells have higher sub-threshold leakage: The reason for a growing importance of subthreshold conduction is that the supply voltage has continually scaled down, both to reduce the dynamic power consumption of integrated circuits and to keep electric fields inside small devices low, to maintain device reliability.

Subthreshold conduction or subthreshold leakage or subthreshold drain current is the current between the source and drain of a MOSFET when the transistor is in subthreshold region, or weak-inversion region, that is, for gate-to-source voltages below the threshold voltage.  

The amount of subthreshold conduction is set by the threshold voltage, which sits between ground and the supply voltage, and so has to be reduced along with the supply voltage. That reduction means less gate voltage swing below threshold to turn the device off, and as subthreshold conduction varies exponentially with gate voltage. So, low-vt cells have higher sub-threshold leakage than high-vt cells*.

4) **Multi Voltage Design -** - Different parts of the chip can have different voltage domains. Having multiple power domains on the same chip introduces some complexities and costs.

**Level Shifter -** Where a logic signal leaves one power domain and enters another, if the voltages are significantly different, a level-shifter cell is necessary to generate a signal with the proper voltage swing. A level shifter cell itself requires two power supplies that match the input and output supply voltages.

***Need to write whether we need level shifer from both low to high and high to low voltage domains or only one***

**Power Switching -** Power switching is a power-saving technique in which portions of the chip are shut down completely during periods of inactivity. Power switching has the potential to reduce overall power consumption substantially because it lowers leakage power as well as switching power. It also introduces some additional challenges, including the need for a power controller, a power-switching network, isolation cells, and retention registers.

A power controller is a logic block that determines when to power down and power up a specific block. There is a certain amount of time and power cost for powering down and powering up a block, so the controller should determine the appropriate power-down times with a high degree of certainty.

*Elaborated details on this power saving technique in another post*.


**Dynamic Voltage and Frequency Scaling -**  The changing of supply voltage and operating frequency during operation to meet workload requirements is called dynamic voltage and frequency scaling.

Dynamic voltage scaling requires a multilevel power supply and a logic block to determine the best voltage level to use for a given task. Design, implementation, verification, and testing of the device can be especially challenging because of the ranges and combinations of voltage levels and operating frequencies that must be analyzed and accommodated.
