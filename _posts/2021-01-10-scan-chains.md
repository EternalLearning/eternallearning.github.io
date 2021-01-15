---
layout: post
title: Scan Chain
---

Scan chains are the elements in scan-based designs that are used to shift-in and shift-out test data. A scan chain is formed by a number of flops connected back to back in a chain with the output of one flop connected to another. The input of first flop is connected to the input pin of the chip (called scan-in) from where scan data is fed. The output of the last flop is connected to the output pin of the chip (called scan-out) which is used to take the shifted data out.

![figure1](https://1.bp.blogspot.com/-ZyhQSc9G_-w/UeXr2KsuTMI/AAAAAAAAAIk/lD8Lk4mr4rM/s640/Scan+chain.png)

**Purpose of scan chain**
Scan chains are inserted into designs to shift the test data into the chip and out of the chip. This is done in order to make every point in the chip controllable and observable as discussed below.

**How normal flop is transformed to scan flop**
The flops in the design have to be modified in order to be put in the scan chains. To do so, the normal input (D) of the flip-flop has to be multiplexed with the scan input. A signal called scan-enable is used to control which input will propagate to the output.

**Purpose of testing using scan**
Scan testing is carried out for various reasons, two most prominent of them are:
- To test stuck-at faults in manufactured devices.
- To test paths in manufactured devices for delay, i.e to test whether each path is working at functional frequency or not.

**How a scan chain works**
The fundamental goal of scan chains is to make each node in the circuit controllable and observable through limited number of patterns by providing a bypass path to each flip-flop. Basically, it follows these steps:
1. Assert scan_enable (make it high) so as to enable (SI -> Q) path for each flop .
2. Keep shifting in the scan data until the intended values at intended nodes are reached.
3. De-assert scan_enable (for one pulse of clock in case of stuck-at testing and two or more cycles in case of transition testing) to enable D->Q path so that the combinational cloud output can be captured at the next clock edge.
4. Again assert scan_enable and shift out the data through scan_out.

**How chain length is decided**
By chain length, we mean the number of flip-flops in a single scan chain. Larger the chain length, more the number of cycles required to shift the data in and out. However, considering the number of flops remains same, smaller chain length means more number of input/output ports is needed as scan_in and scan_out ports. As

Number of ports required = 2 X Number of scan chains

Number of cycles required to run a pattern = Length of largest scan chain in design

Suppose, there are 10000 flops in the design and there are 6 ports available as input/output. This means we can make (6/2=) 3 chains. If we make scan chains of 9000, 100 and 900 flops, it will be inefficient as 9000 cycles will be required to shift the data in and out. We need to distribute flops in scan chains almost equally. If we make chain lengths as 3300, 3400 and 3300, the number of cycles required is 3400.

Keeping almost equal number of flops in each scan chain is referred to as chain balancing.
