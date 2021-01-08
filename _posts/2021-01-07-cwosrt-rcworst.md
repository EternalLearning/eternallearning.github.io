---
layout: post
title: RC Corners
---

RC variation is also considered as corners for the setup and hold checks. RC variation can happen because of fabrication process and the width of metal layer can vary from the desired one.

RC corners have to be split up as per the contribution of each component Ground Capacitance (Cg) and Coupling capacitance (Cc). So on top of the 2 conventional RC corners Cmax and Cmin, foundry came up with 2 more RC corners.

- RC worst (also know as Delay corner) - cc is min, cg*R is max.
- RC best (also known as xtalk corner) - cc is max. cg*R is min.

**cworst corner**
- Refers to corners which result in max cap.
- corner has largest path delay for paths with short interconnect nets.

**RC worst**
- refers to corners which maximize interconnect RC product.
- corner has largest path delay for paths with long interconnects.

**Cbest**
- refers to corners with min cap.
- Interconnect resistance is larger than typical corner.
- results in smallest delay for paths with short nets.

**rcbest**
- refers to corners with min interconnect RC product.
- min resistance and larger than typical cap.
- results in smallest delay for paths with long interconnects.

What’s the Difference between Cworst and RC worst corner?
Cworst- Interconnect capacitance is worst , and in RC worst – product of Interconnect R and Interconnect C become worst (Not like both R & C worst- because that’s can’t possible).
