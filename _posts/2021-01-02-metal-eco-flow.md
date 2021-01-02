---
layout: post
title: Metal ECO flow
---

A metal-only ECO is carried out by changing only metal interconnects in the design. Metal-only ECOs are very common in today’s semiconductor industry as they save complete silicon re-spin. Sometimes there may be need to change the design for various reasons, and that too, a minor change. These changes may be due to some bug in the design or due to customer demand. A metal-only ECO enables the design to be re-fabricated only for a few layers. It is very cost-effective as for complete silicon re-spin, there may be a requirement of around 100 layer masks to be manufactured. Metal-only ECOs enable the older masks to be used for most of the layers. Only the layers with changes in them need to be manufactured again, which is usually 2 to 4 in case of metal-only ECOs.

The steps to carry out metal-only ECOs are explained below:
**1**. A number of spare cells are sprinkled throughout the design before being taped-out so as to facilitate metal layer ECOs later on. The set of spare cells is chosen very carefully considering in mind the nature of design and the probability of metal ECO later on (it depends upon how mature the design building blocks are).

**2**. First, the changes to be made are evaluated if these can be carried out by changing only metal layers. For this purpose, spare cells in the vicinity of the ECO location need to be observed. If there is enough number of spare cells there, these can be used. On the other hand, if there is not enough number of spare cells to represent the logic change, the ECO cannot be carried out using only metal layers. It has to be, then, carried out using all the layers as more cells will need to be added. It will, then, result in re-spin of the design.

**3**. If there is enough number of spare cells available, the appropriate spare cells to represent the design change are selected in the vicinity of the logic to be changed. Interconnects are, then, modified so as to represent the modified circuit.

**4**. The resulting layout is checked for timing and DRC/LVS violations. If everything is fine, the design is sent to be fabricated. There, masks for the modified layers are manufactured using the older masks for layers not modified.

**5**. If there is any violation related to timing or DRC/LVS, steps 2, 3 and 4 are repeated until the design is clean with respect to these.
