---
layout: post
title: EDT Basics
---

## EDT Basics ##
In DFT (Design for Test), EDT (Embedded Deterministic Test) is a compression technology used to reduce test data volume and test time for scan-based testing.
**The core problem it solves:**
Modern chips have millions of scan flops, and traditional ATPG (Automatic Test Pattern Generation) requires loading/unloading full-length scan chains for every test pattern — this creates huge test data volumes and long test times, which get very expensive on ATE (Automated Test Equipment)

**How EDT works:**
On-chip compression/decompression logic is inserted between the chip's scan channels (external pins) and the internal scan chains.
Decompressor: A small number of external input channels feed a decompressor logic block, which fans out and generates data to load many more internal scan chains than there are external pins.
Compactor: On the output side, results from many internal scan chains are compacted back down into a small number of output channels using XOR-based compression logic, before going to the tester.
This is typically built using linear feedback shift registers (LFSRs) or similar linear decompression/compaction networks (Mentor/Siemens Tessent EDT is the most common commercial implementation).

**Benefits:**
Drastically reduces test data volume (fewer bits need to come from the tester)
Reduces test time (fewer clock cycles to shift in/out patterns)
Reduces ATE channel count requirements, lowering test cost
Still achieves high fault coverage comparable to uncompressed scan
