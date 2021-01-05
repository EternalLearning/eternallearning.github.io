---
layout: post
title: What decides target latency and skew numbers?
---
We could also get latency requirements from top level.

Latency depends on the no of flops w.r.t that clock domain and pre-cts logic depths . If you pre-cts logic depth is high (more cts logic in the clock paths because of more controller logic added such as MUX , AOI ) then obviously your latency will be high .Assuming there is less logical cells from the cts root to the flops , the latency depends on the no of flops and the type of floor plan i.e your clock root pin placement and so on.There is no hard and fast rule , however the best possible way is to see how much latency you see when you do only drv fixing on clock paths in  cts .This option is available with the ccopt where its does only drv fixing and not skew. With this you can see the max latency seen from the clock root , if this latency is acceptable  and is required for the drv fixing assuming the placement of the flops is  appropriate  then it is  feasible to set  this as the max latency for your design .

Coming the skew (global)  , you can blindly set it to 10% of clock period for initial analysis  but  is   technology dependent (foundry , PVT , ocv etc) and design dependent  ( ex processor and high speed data paths need very less skew to meet timing) where you cant compromise over the  skew .
