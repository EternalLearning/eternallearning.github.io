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


## from another article

**Synthesis Flow**
Try to use Physical Synthesis as early as possible for better PPA.

**Benefits of early Physical Synthesis**
- Allows synthesis to optimize based on physical constraints from floorplan DEF.
- Use the same floorplan as used in PnR implementation.
- Allows aggressive logic optimizations with realistic floorplan.
- Improves wire dominated paths.
- Generally, CPU designs will use second vt class, and everything else will use third.

Clock gate fanout limit
- Tradeoff between CG count/area and timing/power.
- Typically 32-64 fanout is a reasonable tradeoff.
- Go for more aggressive cloning in case of Htree/Mesh clocking. Consider doing this in innovus (more placement aware cloning).

**PPA Settings in Synthesis**
- Create cost groups for critical modules. Usually critical modules in the design from final (postroute/STA) stage are picked.
- set clock latencies for SRAMs, Architectural ICGs.

**Innovus**
- Standard flow
- Early clock flow.
- Using Early Clock Flow, CTS congestion included (more congestion repair, layer assign sees demand).
- Using Early Clock Flow, better timing correlation (RC sees clock wires, reg2cgate timing).
- Using ECF, better optimization (easy to split flops, skewing knows limit, clock gate enable optimization).

**Power Optimization**
On setting setDesignMode - powerEffort high
- Significant runtime increase for improved power.
- Power Effort high often causes some timing degradation.

On setting power effort to high below events happen
- Leakage recovery.
- Power driven placement.
- Clock skewing for power.
- Power driven routing.
- Full-power aware optimization.
- Dynamic recovery.
- clock power focused ICG placement.

**High Performance CPU flow - high level settings**
For best frequency:
- Use single vt (fastest) through innovus flow.
- powereffort none.
- high optimization.
- place global clock power driven.
- multibitff true.

**Additional settings for best freq and total power (vector based only):**
- Dynamic power optimization.
- power effort high.
- leakagetodynamic 0
- read in activity file
- route with power driven.
- multibitff true.

**Leaakge optimization**
Open all VTs after post-route optimization, and do leakage recovery from tempus eco.

**Congestion**
- Early global route can be optimistic on congested designs. May not model eventual detours effectively, causing timing jumps after CTS/route.
- Model clock net congestion impact. commit_ccopt_clock_tree_route_attributes before place_opt design if not early clock flow.
