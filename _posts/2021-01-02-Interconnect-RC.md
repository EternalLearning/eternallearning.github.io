---
layout: post
title: Interconnect RC
---

Wire delay comes from two sources. One is the intrinsic, speed-of-light delay. The second is the lossy nature of on-chip wires; because the resistance of such wires is very high, the wires form RC circuits. Speed of light delay is proportional to the length of the wire, while RC delay increases with the square of the wire length. Thus, for long wires, RC delay dominates. For shorter wires, speed of light delay still dominates. However, such delay over a short wire is still relatively small compared to gate delay.

In addition to increasing circuit delay, wires are subject to coupling noise. According to Maxwell’s equations, changes in voltages and currents create electric and magnetic fields
which in turn cause changes in voltages and currents on nearby conductors.

We will first examine the RC wire effects which are most important in current circuits. We’ll see how to compute wire resistance and estimate wire capacitance, then use these two properties in wire delay models. We’ll also look at capacitive crosstalk and circuit failures caused by IR drops, electromigration, and self-heating. Since delay and crosstalk are so severe, we will explore how to optimize wire geometries, add repeaters, and drive busses to improve performance.

## Wire resistance

The resistance of a wire can easily be computed from its resistivity ρ, length L, width W, and thickness t:

R = ρL/Wt

Lower metal layers are usually thin, allowing very tight spacing but higher sheet resistance. Metal 1 can be especially thin and resistive. Higher metal layers intended for power distribution and critical signals are thicker and built on a wider pitch, reducing sheet resistance.

## Wire capacitance

The delay of the driver must also be included when estimating the delay of a wire. For example, we can compute the delay of an inverter driving an L mm wire with resistance R/mm and capacitance C/mm, inverter drive resistance Rinv, drain capacitance Cdiff, and total load on the wire Cload.

**To reduce delay, the designer may adjust each of these factors.**

- If the delay is dominated by the wire RC product, increasing wire width is often useful. This decreases wire resistance proportionally, while only slightly increasing total wire capacitance if the capacitance from the top and bottom of the wire is a small part of the whole.
- Increasing spacing between wires is also good because it decreases the total wire capacitance and the amount of coupling to adjacent wires. This is especially important when the adjacent wires might be switching and Miller-multiplying the coupling capacitance.
- Finally, interleaving busses such that the adjacent wires are not switching at the same time prevents Miller-multiplication of coupling capacitance. For example, a bus driven in phase 1 could be interleaved with a bus driven in phase 2 so no two adjacent wires switch at once.


Even when a wire is well optimized to minimize its resistance and capacitance per unit length, wire delay still increases quadratically with distance. At some point, it becomes better to break a long wire in half and insert a buffer, called a repeater, to drive the second half. Each half of the wire contributes 1/4 of the RC delay, thus reducing total RC delay by a factor of 2. The buffer, however, introduces extra delay. The best wire length for introducing a repeater therefore is when the repeater delay is less than the amount of wire delay saved. For a very long lines, breaking the line into more than 2 pieces and adding multiple repeaters becomes beneficial.
