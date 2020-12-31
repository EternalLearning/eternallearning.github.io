---
layout: post
title: MIMCAP and MOMCAP
---

MIM (Metal-Insulator-Metal) and MOM (Metal-Oxide-Metal) capacitors are both metal-to-metal capacitors.

In MIM capacitors, metal plates are stacked on top of each other and separated by a (thin) layer of silicon oxide. Usually this thin oxide is made in a special processing step as the "normal" oxide between metal layers is much thicker (for robustness), which would result in much less capacitance per area. I have seen MIM caps provide around 1-2 femto Farads per square micrometer.

Most MIM capacitors use Metal 5 as the bottom plate, a thin oxide layer, and then a "Metal MIM" as the top plate which is then connected with vias to Metal 6 which will be the usable top plate connection. Direct connections to the "Metal MIM" are not allowed.

**MIMCAP:** formed by two parallel metal layers and has a high k-dielectric between them. This type is most used because of high capacitance per unit area with the lowest parasitics. The drawback is that they require more process steps during the fabrication. Basically, a new mask and a step are added to deposit the insulator between the metal layers.

There may be large current spikes due to simultaneous switching within short periods of time, which can cause the current resistance drop, voltage fluctuation and noise on the power supply network. These will affect reliability, speed and signal integrity. The addition of on-chip decoupling MIMCAP compensates voltage fluctuations by supplying charges to the power network. However, the capacitance must be large enough to meet the requirement.

**MOMCAP:** very similar to MIMCAP but with an oxide layer between metals is usually made by integrating metal layers with the process oxide. They have lower capacitance per unit area, but they are cheaper.
