---
layout: post
title: Different Setup and Hold fix methods!
---

## Ways to fix Setup and hold timing violation
**Setup** time is defined as the minimum amount of time before the clock's active edge by which the data must be stable for it to be latched correctly. Any violation in this required time causes incorrect data to be captured and is known as a setup violation.

**Hold** time is defined as the minimum amount of time after the clock's active edge during which data must be stable. Violation in this case may cause incorrect data to be latched, which is known as hold violation.

![Flops](https://www.edn.com/wp-content/uploads/media-1168670-setupandhold1.jpg)

![Waveforms](https://www.edn.com/wp-content/uploads/media-1168671-setupandhold2.jpg)

The setup time requirement equation is as below:

Tc-q + Tcomb + Tsu <= Tclk + Tskew

Tc-q + Tcomb >= Thold + Tskew

**Ways to address Setup violation:**

1) First check the launch and capture flop placement. If they seem to be very far off, we need to group them for closer placement. Once we confirm that the two flops are at reasonable distance we need to understand more about the data path delays first.

2) Check for the cell delays in the combination path. If any of the cell delay seem to be an outlier investigate more and try to reduce the cell delay.

3) Remove some back to buffers if the overall stage delay decreases by doing so. That is, removing some buffers might reduce cell delay but increase wire delay. So, we have to be cautious and remove buffers only if cell delay + wire delay is decreased by doing so. This method is more useful where the net length is small.

4) Increase the drive strength (that is upsize the cells). <br>
Explanation: Let us say we upsize the basic inverter (2:1) by 2x that is 4:2. The output capacitance may charge and discharge twice as fast as the basic inverter because the Ron resistance is now divided by 2. (Pmos in parallel). Thus increasing the drive strength speeds up the cell.

5) Change cell type in combo path to LVT. <br>
Explanation: Low Vt decreases the transition time and overall propagation delay through that cell decreases. Foot print swapping, that means the size of the cell for HVT/LVT remains the same. This comes in handy especially when the project is nearing the deadline. The leakage current and speed both are high in LVT cells compared to HVT cells. LVT > NVT > HVT.

6) Check for net routing , make sure there is no detour in the routing. Also try to route on higher metal layers if there is long route or if there is a congestion on lower layers. upper layers have less resistance. **Upper metal layers are thicker and hence capacitance is lesser thus net delay reduces.**

7) Insert buffers where there is a long wire. <br>
Explanation: Sometimes we insert buffers if the net is really long. Inserting buffer on a long net reduces the transition time, which decreases the wire delay and overall stage delay.


8) Adjust cell position in layout if necessary.

9) See if there is a possibility to add two inverters instead of a buffer. Doing so might reduce the overall stage delay. Adding inverter, reduces the stage delay by two times. And this method will be more impactful if the wires before and after the original buffer are very long. That way we reduce the wire delay by adding another inverter on the net and changing the buffer to inverter.

10) Check for cross talk and fix if existing.

11) After trying all possible ways to reduce the combo delay and ran out of ideas to extract further timing from data delay then we can explore tweaking clock. By delaying the clock to the endpoint, we can relax the setup timing of the path. We need to make sure the downstream setup paths are not critical and this path do not have critical hold time. This is also termed as useful skew.

12) Try to balance launch and capture flop insertion delays.


**Ways to address hold violation:***

1) Add more data path delay. End point buffering is usually preferred as that won't affect the previous and next stage.

2) Downsize the datapath cells to increase the transition time which eventually increases the cell delay.

3) Change the cell type to HVT.
