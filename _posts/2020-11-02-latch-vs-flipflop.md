---
layout: post
title: Latch vs Flip Flop
---

**Latch**
- Latch is faster (no need to wait for clock edge) but less predictable (more prone to race conditions).
- Latch uses less area (becasue there are less no.of gates).
- Latch is fast (the longer combinational path could be compensated by shorter paths in the subsequent logic states). That is why for high performance, circuit designers are turning into latch based design.
- For ASIC's with large skew, latches have substantial benefits for reducing the clock period.

**Disadvantages of latch**
- Latch based design is noisy, because any noise in the enable signal disrupts the latch output easily whereas FF based design is robust.
- Latch based designs are difficult to analyze due to level sensitive property.
