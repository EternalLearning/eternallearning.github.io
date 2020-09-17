---
layout: post
title: Skew groups
---

The feature of balancing more than one group of clock pins is similar to the capability of local  skew  or  useful  skew  in  clock  tree  synthesis. For  example,  you  can  group  clock  sinks  that  are  not  related  to  timing  critical  paths  in  a  skew  group  and  relax  the  target  skew goals of the skew group. You can also give separate goals of target early delay to different skew groups and achieve the similar effect of useful skew to improve the slack of critical timing paths.  

There are three types of skew groups:

1. Simple fanout tree in a clock domain. There might be buffers or logic gates  between  the  clock  root  and  the  sinks,  but  the  sinks  can  be  easily  grouped  separately into skew groups.

2. The second type is to balance the clock pin of an integrated clock-gating cell with a flip-flop  to  satisfy  the  timing  requirement  of  the  critical  path  between  the  flip-flop  and  the  integrated clock-gating cell. Without the skew group feature, the clock pin of integrated clock-gating  cell  will  not  be  considered  as  an  endpoint  for  balancing.  Thus,  controlling  the arrival time of the clock pin of the integrated clock-gating cell relative to the flip-flop to satisfy the critical path timing is difficult.

3. The third type, as shown below, is similar to the previous type. You need to balance the endpoint with the clock pin of the flip-flop where the generated clock starts to satisfy the timing  requirement  of  the  critical  path  between  the  flip-flop  and  the  generated  clock.  Likewise, without the skew group feature, the clock pin of the generated clock flip-flop is not treated as an endpoint for balancing. Thus, controlling the relative arrival time of the generated  clock  source  relative  to  the  flip-flop  to  satisfy  the  critical  path  timing  is  very  difficult.

some useful commands:
set_skew_group
report_skew_group
report_clock_tree -skew_group


### Guidelines for defining skew groups
- Do not include hierarchical pins in a skew group.
- Pins that can be included in a skew group are nonhierarchical pins, such as leaf stop pins, leaf float pins, the clock pins of integrated clock-gating (ICG) cells, and clock pins that start the sequential timing arcs.
- Include only pins from the same master clock domain in a skew group.
- Each pin can only belong to one skew group.
- Only  one  level  of  dependency  is  allowed  between  skew  groups. For  example,  skew  group  A  is  dependent  on  skew  group  B,  so  no  skew  groups  should  be  dependent  on  skew group A.
