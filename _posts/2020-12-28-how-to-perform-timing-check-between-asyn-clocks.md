---
layout: post
title: How to perform timing check between asynchronous clock domains
---

If two clock domains are asynchronous and you have applied set_false_path between these two clocks, no timing checks can be performed. Also, if you have defined a clock group with asynchronous clocks using the set_clock_groups command​ with the -asynchronous option, by default the tool cannot perform a timing check. But if you use the -allow_paths option with the set_clock_groups command, timing check can be performed.

To control the data path delay between the registers from asynchronous clock domains:

1. Specify the asynchronous clock domains such that the timing check is possible. So, the asynchronous clock domains should be specified with the set_clock_groups constraint, not with the set_false_path constraint.

2. Select appropriate SDC constraints to restrict the combinational delay. So, the set_clock_groups constraint should be specified with the -asynchronous and -allow_paths options, as depicted in the following example:

set_clock_groups -asynchronous -group [get_clocks clk1] -group [get_clocks clk2] -allow_paths

After enabling timing check on the domain crossing paths, use the set_max_delay timing constraint with the -combinational_from_to option to specify the maximum delay allowed on this path segment.

# Style 1: delay constraint using clock references
set_max_delay <maximum delay value> -from [get_clocks clk1] -to [get_clocks clk2] -combinational_from_to

# Style 2: delay constraint using references to registers at clock domain crossing
set_max_delay <maximum delay value> -from <clock pin of source register FF1/CK> -to <data pin of destination register FF2/D> -combinational_from_to

The -combinational_from_to option guarantees the following:

- Only the combinational path delay check is performed at the clock domain crossing.
- Removes clock latencies from the delay computation.
- Removes setup/hold constraint of the capture register from the delay computation.

Note: Append the -path_exceptions all option to the report_timing command to diagnose the max delay constraint applied on the path

To summarize:

- Use set_clock_groups -asynchronous -allow_paths for asynchronous clock specification.
- Use set_max_delay -combinational_from_to for path delay specification.

## How to apply set_max_delay constraint path without clock latencies
If I apply set_max_delay on a Reg2Reg path and run report_timing, it generates the timing report including launch and capture clock latencies.

> set_max_delay -from [get_clocks CLKB] -to [get_clocks CLKC] 2
> report_timing -from [get_clocks CLKB] -to [get_clocks CLKC] -path_type full_clock

Path 1: VIOLATED Setup Check with Pin FF2/CK
Endpoint:   FF2/D (^) checked with  leading edge of 'CLKC'
Beginpoint: FF1/Q (^) triggered by  leading edge of 'CLKB'
Path Groups: {CLKC}
Other End Arrival Time          1.767
- Setup                         0.179
+ Path Delay                    2.000
= Required Time                 3.588
- Arrival Time                  8.883
= Slack Time                   -5.295

    Clock Rise Edge                      0.000
     = Beginpoint Arrival Time            0.000

  Timing Path:
     ---------------------------------------------------------------------------
      Timing Point  Edge  Cell     Delay  Arrival  Required  Slack   Phase
                                          Time     Time               
      ---------------------------------------------------------------------------
      clkB          ^     -        -      0.000    -5.295    -5.295  CLKB(C)(P) *
      C1/Y          ^     MX2X1    0.201  0.201    -5.093    -5.295  CLKB(C)(P) *
      C2/Y          ^     BUFX1    1.679  1.881    -3.414    -5.295  CLKB(C)(P) *
      FF1/Q         ^     DFFHQX1  1.914  3.794    -1.501    -5.295  CLKB(D)(P)  
      U1/Y          ^     BUFX1    1.649  5.443    0.148     -5.295  CLKB(D)(P)  
      U2/Y          ^     AND2X1   1.814  7.258    1.963     -5.295  CLKB(D)(P)  
      U3/Y          ^     BUFX1    1.622  8.880    3.585     -5.295  CLKB(D)(P)  
      FF2/D         ^     DFFHQX1  0.003  8.883    3.588     -5.295  CLKB(D)(P)  
      ---------------------------------------------------------------------------

     Clock Rise Edge                      0.000
     = Beginpoint Arrival Time            0.000
     Other End Path:
      ---------------------------------------------------------------------------
      Timing Point  Edge  Cell     Delay  Arrival  Required  Slack   Phase
                                          Time     Time               
      ---------------------------------------------------------------------------
      clkC          ^     -        -      0.000    5.295     -5.295  CLKC(C)(P) *
      C3/Y          ^     MX2X1    0.101  0.101    5.396     -5.295  CLKC(C)(P) *
      C4/Y          ^     BUFX1    1.662  1.764    7.059     -5.295  CLKC(C)(P) *
      FF2/CK        ^     DFFHQX1  0.003  1.767    7.062     -5.295  CLKC(C)(P) *

      ---------------------------------------------------------------------------

I want to apply the set_max_delay constraint without launch and capture latency from Q pin to D pin of a Reg2Reg path triggered by specified clocks. How can I do this?

## Answer
You can use the -combinational_from_to option with the set_max_delay command to treat path -from / -to pins as purely combinational; that is, both the source/capture clock latencies are excluded from slack computation as shown in the following example.

> set_max_delay -from [get_clocks CLKB] -to [get_clocks CLKC] 2 -combinational_from_to
> report_timing -from [get_clocks CLKB] -to [get_clocks CLKC] -path_type full_clock

Path 1: VIOLATED Path Delay Check

Endpoint:   FF2/D (^)
Beginpoint: FF1/Q (^) triggered by  leading edge of 'CLKB'
Path Groups: {CLKC}
Path Delay                      2.000
= Required Time                 2.000
- Arrival Time                  6.999
= Slack Time                   -4.999

     Clock Rise Edge                  -
     = Beginpoint Arrival Time       0.000
      ---------------------------------------------------------------------------
      Timing Point  Edge  Cell     Delay  Arrival  Required  Slack   Phase
                                          Time     Time               
      ---------------------------------------------------------------------------
      FF1/CK        ^     -        -      0.000    -4.999    -4.999  CLKB(C)(P) *
      FF1/Q         ^     DFFHQX1  1.910  1.910    -3.089    -4.999  CLKB(D)(P) *
      U1/Y          ^     BUFX1    1.649  3.559    -1.440    -4.999  CLKB(D)(P) *
      U2/Y          ^     AND2X1   1.814  5.374    0.374     -4.999  CLKB(D)(P) *
      U3/Y          ^     BUFX1    1.622  6.996    1.997     -4.999  CLKB(D)(P) *
      FF2/D         ^     DFFHQX1  0.003  6.999    2.000     -4.999  CLKB(D)(P) *
      ---------------------------------------------------------------------------
