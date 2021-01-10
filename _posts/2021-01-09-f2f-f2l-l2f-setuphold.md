---
layout: post
title: Setup/hold on Flop2flop, flop2latch and latch2flop
---

## Positive edge triggered FF to positive level sensitive latch

![figure1](https://4.bp.blogspot.com/-YlbCt1C5-A0/Ug5Xt_qOp1I/AAAAAAAAALw/6edpLewcXys/s640/setup+and+hold+for+positive+edge+flop+to+positive+level+latch.png)

![figure2](https://2.bp.blogspot.com/-V_vABv7rbj4/VemfCosP_lI/AAAAAAAAAIE/C4EFSzJ5wBI/s640/pos_flop_to_pos_latch.png)

Tck->q + Tprop + Tsetup < Tskew + Tperiod/2  (for setup)

Tck->q + Tprop > Thold + Tskew  - (Tperiod/2)   (for hold)


## Positive edge triggered FF to negative level sensitive latch

![figure3](https://3.bp.blogspot.com/-Sd9kgawNvtg/Ug5XrnnEbLI/AAAAAAAAALM/PV4XiQNGznw/s640/setup+and+hold+check+for+positive+edge+flop+to+negative+level+latch.png)

![figure4](https://3.bp.blogspot.com/-ROCDyspQbqg/VemfUQIAamI/AAAAAAAAAIM/qkNO50svBn0/s640/pos_flop_to_neg_latch.png)

Tck->q + Tprop + Tsetup < Tskew + Tperiod (for setup)

 Tck->q + Tprop > Thold + Tskew  (for hold)

## Negative edge triggered FF to positive level sensitive latch

![figure5](https://3.bp.blogspot.com/-2D9FBg_fyU4/Ug5XradhLSI/AAAAAAAAALE/SpYu6LYjT6w/s640/setup+and+hold+check+for+negative+edge+flop+to+positive+level+latch.png)

![figure6](https://2.bp.blogspot.com/-NXpT2gIz8o8/VemfjSM-22I/AAAAAAAAAIU/OMySjz4cktI/s640/neg_flop_to_pos_latch.png)

Tck->q + Tprop + Tsetup < (Tperiod) + Tskew (for setup)

 Tck->q + Tprop > Thold + Tskew  (for hold)

## Negative edge trigerred FF to negative level sensitive latch

![figure7](https://1.bp.blogspot.com/-dqB3Fl9DOWE/Ug5Xrf1_T-I/AAAAAAAAALI/fnBOVd9DrZk/s640/setup+and+hold+check+for+negative+edge+flop+to+negative+level+latch.png)

![figure8](https://3.bp.blogspot.com/-4xIHxEIPRDk/VemfyDCR30I/AAAAAAAAAIc/cEn1aWlkxfU/s640/neg_flop_to_neg_latch.png)

Tck->q + Tprop + Tsetup < Tskew + Tperiod/2  (for setup)

Tck->q + Tprop > Thold + Tskew  - (Tperiod/2)   (for hold)
