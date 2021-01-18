---
layout: post
title: Negative net delay cases
---

In some cases tool reports negative net/cell delay. The delay for a net/cell is the time it takes for the output signal to reach threshold point after the input signal reaches to threashold point.

In most cases the delay is the time taken for input signal to reach 50% of input pin transition to the output signal to reach 50% of output transition.

This delay could be negative if the output transition reaches to its threshold point before input transition reaches to its threshold point due to any of the following reasons.

- First, If there is difference in the driver and receiver voltage. The voltage of receiver is lower compared to driver, and the delay is measured at lower voltage. Hence, for rise transition, if the wire delay is not significant the delay will become negative due to the diff in measurement voltage.
- Second, if the input transition is very large and output transition is short, which mean there is improvement in slew. Generally, the slew degrades due to net parasitics. However, slew can be improved if there is crosstalk, as crosstalk can improve the slew and may result in negative delay.
- Third, If there is a noisy waveform at the input of the receiver cell.

## Why does tool report zero delay for cells?
1. During ECO, if timing update is turned off, the cells that have been added by ECO command all have zero delay. By default, ECO commands will update timing graph after each ECO.
2. If you specify set ideal network on port or driver pin, it will mark all pins and nets of its transitive fanout as ideal.

## Reasons for infinite timing window during SI analysis
1. If set si mode -use infinite tw is true
2. If the net reselected in later iterations. SI analysis starts with infinite TW in the first itertion and TW is converged in later iterations. There can be situations where some nets are not reselected in second iteration due to the criteria speficied using set si mode si reselection slack and for such nets the TW will be considered as infinite.
3. Victim and attacker constrained by asynchronous clocks.
4. The net is unconstrained and set si mode -unconstrained net use inf twf true is used. An attacker net can be considered as unconstrained if the net is the fanout of :
- input ports without any input delay specified.
- pins specified with -to option of set disable timing
- unclocked registers Q pins
- output pin of a register clocked by constant.
