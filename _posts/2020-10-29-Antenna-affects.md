---
layout: post
title: Antenna Affects
---

Effect of charge accumulation in isolated nodes of an integrated circuit during its processing is known as Antenna effect. This effect is also known as Plasma Induced Damage. The discharging of accumulated charges, which is done through the thin gate oxide of the transistor, it might cause damage to the transistors and degrade its performance.

During the fabrication process, we need to etch out the unwanted oxide layer from the wafer, which can be done using plasma etching. Plasma contains high energetic ions and radicals for etching that get collected by interconnect. The amount of charge accumulation depends on the surface area of interconnects. These collected ions increase the potential of interconnects and if the interconnect is connected to gate, a drainage path could be formed through the gate oxide to balance out the charge collection depending upon the amount of charge collected by interconnects. The drainage path could either breakdown the gate oxide, which may lead to permanent damage of the device. During the plasma etching of interconnect, positive ions and neutral species from the plasma get strikes at interconnector these are collected by the interconnects, so if the area of the interconnect connected to the gates is large enough then the potential of the interconnect or poly silicon maybe large enough that it could breakdown the gate oxide.

Foundry provides the antenna rule file, which must be followed during the layout design. In the antenna rules most common rule is Antenna Ratio. Antenna ratio is the ratio of metal area connected to the gate to the total area of gate.Antenna area/gate area < Maximum Antenna Ratio

## Techniques to fix antenna violations.
**Routing on Higher Metal Layer:** Long metal can be taken to higher metal routing layer. This is known as metal jumping. This metal jumping is usually done near to the load. This metal jumping will break the long interconnect and hence the charge collected on the long interconnect will not discharge through gate oxide because the higher metal layer is not yet fabricated. This solution may increase the routing congestion on higher metal layer.

**Reduce the via-area** - Large via area also results in process antenna violation. Converting multi-cut vias to double-cut via or double-cut vias to single-cut via reducing the cut area. This may impose serious reliability issues such as electro migration.

**Diode Insertion** Diode helps dissipate charges accumulated on metal. Diode should be placed as near as possible to the gate of device on low level of metal. Connecting a diode to the gate electrode which provides a discharging path for the static charge present on the metal layer. Diode should always be connected in reverse bias, with cathode connected to gate electrode and anode connected to ground potential.

## And by adding diode, routing to upper metal layer and reducing via area we can solve the antenna violation.

A very thin gate oxide is less likely to be damaged than a thick gate oxide, because as the oxide grows thinner, the leakage goes up exponentially, but the breakdown voltage shrinks only linearly.

If Antenna Violstion happens at a metal layer, always use higher metal layers to jump since all lower metal layers are already fabricated.
