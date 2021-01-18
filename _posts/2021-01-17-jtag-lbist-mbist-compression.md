---
layout: post
title: Jtag, LBIST, MBIST, compression
---

**LBIST**
The LBIST (Logic built in self test) is inserted into a design to generate patterns for self-testing.

**JTAG/Boundary Scan**
Method for testing interconnects on printed circuit board or sub blocks inside an IC. JTAG developed a specification for boundary scan testing that was standardized in 1990.

**MBIST**
MBIS is a self testing and repair mechanism which tests the memories through an effective set of alogorithms to detect possibly all the faults that could be present inside a typical memory cell whether it is stuck at , transition delay faults.

**Scan compression and decompression**
Test compression is a technique used to reduce the time and cost of testing integrated circuits. The first ICs were tested with test vectors created by hand. It proved very difficult to get good coverage of potential faults, so Design for testability (DFT) based on scan and automatic test pattern generation (ATPG) were developed to explicitly test each gate and path in a design. These techniques were very successful at creating high-quality vectors for manufacturing test, with excellent test coverage. However, as chips got bigger the ratio of logic to be tested per pin increased dramatically, and the volume of scan test data started causing a significant increase in test time, and required tester memory. This raised the cost of testing.

Test compression was developed to help address this problem. When an ATPG tool generates a test for a fault, or a set of faults, only a small percentage of scan cells need to take specific values. The rest of the scan chain is don't care, and are usually filled with random values. Loading and unloading these vectors is not a very efficient use of tester time. Test compression takes advantage of the small number of significant values to reduce test data and test time.

![figure1](https://i1.wp.com/semiengineering.com/wp-content/uploads/2019/09/Cadence_Scan-Compression-fig1-two-scan-chains-4x-compression.png?w=1360&ssl=1)

As design size increased, the I/O interface did not scale with the increasing FFs. As a result, the scan chains became too long. Test time and data volume of scan-based tests is also shown in Figure 1. Both the test time and data volume are linearly dependent on the chain length.

Scan compression relies on breaking the link between the scan I/O and the scan chains such that many more internal scan chains can be constructed making the chain length shorter. By doing this, the test time is targeted to be 4X less than that of scan design.

Once the link between the scan I/Os and the internal scan chains is broken, the remaining problem to be solved by the scan compression technology is to determine the interfacing logic between the few scan I/Os and the many internal scan chains. A simple example of a scan compression architecture fans out the scan inputs to the internal scan chain inputs and xors the internal scan chain outputs to connect them to the design scan outputs. The logic on the scan input side is called the decompressor, and the logic on the scan output side is called the compressor.

Compression and decompression circuits between the chain I/Os and the scan I/O pins ensure that the number of scan I/O pins remains the same. Sharing of internal inputs to the scan chains means that the number of bits in each scan ATPG pattern is reduced by the same factor, x.  Likewise, reducing scan depth by the same factor makes it possible to scan in and test x times more patterns in the same amount of time.

Scan compression relies on creating high fan-out connections or brings together widely dispersed scan chains to few scan-outs.The main problem with scan compression and decompression is increase in wire length.
