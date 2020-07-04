---
layout: post
title: Flat vs Hier Designs!
---

## Introduction
The disadvantages of flat runs are more run times, more memory requirement, limitation of EDA tools to handle designs greater than certain gate count.The design under discussion had two blocks. So effectively we had two blocks and a chip_top to pay attention to. Figure 1 is the floorplan in such a case. As you can see, there are two huge partitions in the design. It can also be seen that there is huge channel in between the blocks. Also, there are certain design requirements like signals from either of the partitions should not cross over the other partition
