---
layout: post
title: Time borrowing for latch based designs
---

For Flops, data arrival later than capture clock edge causes SETUP violation. Whereas Latch remains transparent for entire duration of active clock edge, relaxing arrive-before-edge criterion.

As a result of this feature in latches:

- Data can arrive later than capture clock arrival and borrow from the next clock cycle. This is called **Time Borrowing or Cycle SStealing** and the current stage slack improves.
- Time available for next stage reduces, but no adverse effect if next-stage delay is short.

There are two kinds of time borrowing modes:

1) Max Time Borrow (default)

2) Latch Through Mode (this can be enabled if there are consecutive latches in the path and they are borrowing time)

## Time borrow Analysis
Maximum borrow time = earliest latch open edge - latest latch closing edge.

**Factors affecting Edge positions of capture clock**
- Library setup constraint of endpoint latch.
- open vs close edge clock network latency.
- open vs close edge clock uncertainty and jitter
- open vs close edge CPPR

CPPR reduces pessimism while uncertainty(skew) and jitter increases it.

Max Borrow Time = w - s - deltaL - deltaCPPR + deltaUncert + deltaJitter

w - pulse width of the capture clock
s - library setup constraint of endpoint latch
deltaL - clock latency of latch open edge - clock latency of latch close edge
deltaUncert - clock uncert of latch open edge - clock uncert of latch close edge
deltaCPPR - CPPR for latch open edge - close edge
deltaJ - latch open edge J - close edge J
