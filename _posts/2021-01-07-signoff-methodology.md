---
layout: post
title: Signoff Methodology
---

STA can be run for many different scenarios. The three main variables that determine a scenario are:
- Parasitic corners (RC interconnect corners and operating conditions used for parasitic extraction).
- operating mode.
- PVT corner.

## parasitic interconnect Corners
Parasitics can be extracted at many corners. These are mostly governed by the variations in the metal width and metal etch in the manufacturing process. Some of these are:
-  Typical: This refers to the nominal values for interconnect resistance and capacitance.
- Max C: This refers to the interconnect corner which results in maximum capacitance. The interconnect resistance is smaller than at typical corner. This corner results in largest delay for paths with short nets and can be used for max path analysis.
- Min C: This refers to the interconnect corner which results in minimum capacitance. The interconnect resistance is larger than at typical corner. This corner results in smallest delay for paths with short nets and can be used for min path analysis.
- Max RC: This refers to the interconnect corner which maximizes the interconnect RC product. This typically corresponds to larger etch which reduces the trace width. This results in largest resistance but corresponds to smaller than typical capacitance. Overall, this corner has the largest delay for paths with long interconnects and can be used for max path analysis.
- Min RC: This refers to the interconnect corner which minimizes the interconnect RC product. This typically corresponds to smaller etch which increases the trace width. This results in smallest resistance but corresponds to larger than typical capacitance. Overall, this corner has the smallest path delay for paths with long interconnects and can be used for min path analysis.
