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

Magnitude of the glitch caused is dependent upon a variety of factors.

1. **coupling capacitance between the aggressor net and victim**: the greater the cc, the larger the magnitude of the glitch.
2. **slew of the aggressor net:** faster the slew at the aggressor net, the larger the magnitude of glitch. In general, faster slew is because of higher output drive strength for the cell driving the aggressor net.
3. **victim net grounded capacitance:** the smaller the grounded capacitance on the victim net, the larger the magnitude of the glitch.
4. **victim net driving strength**: the smaller the output drive strength of the cell driving the victim net, the larger the magnitude of the glitch.

DC and AC thresholds
The DC noise analysis only examines the glitch magnitude and is conservative whereas AC noise analysis examines other attributes such as glitch width and fanout cell output load.

In general, a single stage cell will stop any input glitch which is much narrower than the delay through the cell. This is because with a narrow glitch, the glitch is over before the fanout can respond to it. Thus, a very narrow glitch does not have any effect on the cell. Since the output load increases the delay through the cell, increasing the output load has the effect of minimizing the impact of glitch at the input though it has adverse effect of increasing the cell delay.

For a given cell, increasing the output load increases the noise margin since it increases the inertial delay and the width of the glitch that can pass through the cell. Thus, increasing the load at the output makes the cell more immune to noise propagating from the input to the output.

For multiple aggressors, the use of timing windows reduces the pessimism in the analysis by considering the switching window during which a net can possibly switch.


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

## LVF characterization
LVF variation models contain a large amount of statistical variation information, unlike nominal value timing models. Because of that, characterizing LVF models requires Monte Carlo analysis, resulting in a lengthier characterization process. Using brute-force Monte Carlo to characterize all LVF values in a .lib would result in thousands of Monte Carlo SPICE simulations for each table entry, increasing characterization runtime by several orders of magnitude. For obvious reasons, this is not a feasible approach for characterizing entire .libs.

To make LVF characterization feasible, characterization tools use various techniques, such as netlist reduction and sensitivity-based approximations, to reduce runtime. These approximations are able to reduce runtimes to “only” 5x-10x of nominal timing characterization runtimes. However, these approximations often also introduce inaccuracies to the resulting .libs, leading to incorrect static timing analysis (STA) results and potentially causing silicon failure.

A comprehensive and reliable methodology to validate LVF data is crucial in today’s design flows. Without this step, the design team can be exposed to faulty or noisy LVF values that may sway timing results by 50%-100% outside of production-accurate ranges, resulting in timing closure issues and delays and potential silicon failure.
