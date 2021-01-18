---
layout: post
title: Glitch noise analysis and crosstalk analysis
---

Glitch noise validates integrity of steady signal in presence of various noise sources and its impact on downstream circuit.

## What is glitch violation
Approach #1a: Use receiver input peak (RIP). Compare RIP to RIP threshold. not accurate, pessimistic.
Approach #1b: Use noise immunity curver, f(Hrip Wrip) > thresholdRIP. Not accurate, obsolete now.

Approach #2: Use Receivers first stage output peak (ROP). Simulate ROP using CCSN and actual output load. Check ROP peak against Threshold ROP.

Analysis is done on a stage:
- includes driver and receivers of victim, aggressor net and coupled RC network.
- computes glitch at each receivers input (RIP).
- criticality is then determined based on ROP glitch.

Major factors affecting RIP glitch magnitude and a failure decision:
- No. of aggressors.
- coupling cap.
- slew rate.
- victim driver cell.
- driver weakening.
- receiver cells.
- Their loads.

**Glitch Analysis**
We can perform two types of glitch analysis:
1. When we do not have noise libraries: in this case, glitch analysis will take place but will not be accurate. Also, the glitch will be calculated only at input of receiver, which is RIP.
2. When we have noise libraries, in this case, glitch analysis will be more accurate. Glitch calculated at the input of receiver will be propagated inside the cell and ROP is calculated.

- Library reading with or without noise (CCS-T or CCS-N)
- aggresor selection: TW determination
- glitch settings: threshold


## Cross talk analysis
- we can separate delta delay on data (delta delay is always separated for clock ).
- We shld specify delta delay annotation mode (either lumped on net or arc). lumps the delata delay all onto the net if it is broken out. Arc, separates the delata delay onto each cell arc and net arc this is default.
- No. of iterations, Specifies the maximum number of timing window iterations that should be performed during SI delay analysis. Performing delay analysis results in changes to the timing windows, which can potentially change the victim/attacker net combinations. Timing window iterations enable the software to reach to a point where the delay changes are negligible.
- SI reselection mode, Specifies the SI reselection criteria during timing window iterations. Delta Delay: Reselects a net for calculation if the delta delay on the net is greater than the delay value. Slack: Reselects nets that have failing slacks during timing window iterations.
- Start SI iterartion at: Nominal: During timing window iterations, uses nominal timing windows first. Infinite: During timing window iterations, uses infinite timing windows first.
