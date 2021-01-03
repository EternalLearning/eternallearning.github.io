---
layout: post
title: PPA tips and tricks
---

Performance/Power/Area are the key metrics to validate the functionality of any design on a given technology node. In this article we will go over various place-and-route techniques to achieve these key metrics.

At Placement stage one need to make sure overall quality of the design improves instead of looking at the individual paths or individual power metrics.

First and foremost thing to do before starting any design exercise or running through backend flows, check the quality of the design inputs (Please see, Quality and inputs section) and then identify if the design is critical for timing (frequency), power or area.

Once the most important criterion is identified, keep the other aspects constant and experiment with most challenging aspect first and try to iterate one at a time to understand design behavior.

## Common Settings for PNR
1. MBFF can be enabled for better area and leakage convergence, though with some penalty on timing. It is advised to enable MBFF on all flops in the first run
and later remove timing-critical flops from being MBFF.

2. Power effort needs to be set for each stage; low/high/ultrahigh settings are to be used to converge on power. Use the leakageToDynamicRatio switch to let
the tool focus on leakage/dynamic or on both with the same effort. Dynamic power optimization requires a TCF file.

3. Useful skew can be used during optimization to fix setup. Make the settings below before any optimization.

4. Uncertainty: Keep the right uncertainty at place and gradually reduce it at CTS and at route. Signoff stage should have signoff uncertainty applied. The typical
formula to calculate uncertainty at place is “Twice of signoff uncertainty + some margin”.
