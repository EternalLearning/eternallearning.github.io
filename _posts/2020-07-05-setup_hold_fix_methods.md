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

**Ways to address Setup violation:**

1) First check the launch and capture flop placement. If they seem to be very far off, we need to group them for closer placement. Once we confirm that the two flops are at reasonable distance we need to understand more about the data path delays first.

2) Check for the cell delays in the combination path. If any of the cell delay seem to be an outlier investigate more and try to reduce the cell delay.

3) Increase the drive strength (that is upsize the cells). <br>
Explanation: Let us say we upsize the basic inverter (2:1) by 2x that is 4:2. The output capacitance may charge and discharge twice as fast as the basic inverter because the Ron resistance is now divided by 2. (Pmos in parallel). Thus increasing the drive strength speeds up the cell.

4) Change cell type in combo path to LVT. <br>
Explanation: Low Vt decreases the transition time and overall propagation delay through that cell decreases. Foot print swapping, that means the size of the cell for HVT/LVT remains the same. This comes in handy especially when the project is nearing the deadline. The leakage current and speed both are high in LVT cells compared to HVT cells. LVT > NVT > HVT.

4) Check for net routing , make sure there is no detour in the routing.

5) Insert buffers where there is a long wire.

6) Adjust cell position in layout if necessary.

7) After trying all possible ways to reduce the combo delay and ran out of ideas to extract further timing from data delay then we can explore tweaking clock.By delaying the clock to the endpoint, we can relax the setup timing of the path. We need to make sure the downstream setup paths are not critical and this path do not have critical hold time. This is also termed as useful skew.
