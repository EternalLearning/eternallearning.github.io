---
layout: post
title: PPA comparision of 8kx32 vs two 4kx32 SRAMS
---

Let us first compare the power between two designs. For 8kx32 sram, there will be one clock read and 1 clock write. But if we divide that into two then we need to have more clocks and we might see more power dissipated in the second design than first.

**Performance** wise

**Area** wise we need to built clock to 2 srams reset to 2 srams so we may end up having more area used than the first design. 
