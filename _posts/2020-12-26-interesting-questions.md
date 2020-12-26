---
layout: post
title: Gate vs interconnect delay
---

Problem Statement: One path has 100% gate delay, second path has 50% gate delay and 50% interconnect delay, third path has 100% interconnect delay. These 3 different paths converged at a voltage and temperature. What would happen to each path if voltage and temperature changed? Which would be fast and which would be slow?

Reasoning:

For a cell, we could see variation due to process and cell delay decreases with increase in supply voltage. Cell delay increases with increase in temperature but at lower tech node, even when temperature decreases we see delay increase.

For a wire (RC), process variation is seen, no voltage dependance for R and C. Temperature dependance seen in resistance. Increase in temperature increases resistance. Temperature dependance not seen in capacitance. There is no change in net delay from higher tech node to lower tech node wrt temperature.

## Voltage increased:
Path1: Cell delay reduces with increase in voltage. So, overall delay <100ps. potentially, this path could be faster.
Path2: RC is not dependant on voltage, so only <50ps part is reduced. This path could be faster than path3 but not as fast as path1.
Path3: RC is independent of voltage. So, the delay remains to be 100ps. No delay change in this path and since path 1 and 2 are faster now, comparatively, path3 is slower.

## Voltage decreased:
Path1: With decrease in voltage, delay increases, >100ps. This path becomes slower.
Path2: With decrease in voltage delay increases, >50ps + 50ps.This is slower compared to path3 but could be faster than path1.
Path3: No change in delay. This path will become faster when compared to path1 and path2.

## Temperature increased:
Path1: Delay is increased in cell.  
Path2: Delay is increased in cell and net. This could be the slowest path now. This is slowest
Path3: Delay is increased in net.

## Temperature decreased:
Path1: Delay is increased in cell due to temperature inversion.This path is slowest.
Path2: Delay is increased in cell due to temperature inversion.
Path3: Delay is decreased in net but increased in cell. This path is fast.
