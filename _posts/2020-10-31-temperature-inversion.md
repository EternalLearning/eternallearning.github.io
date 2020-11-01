---
layout: post
title: Temperature Inversion
---

In all, there are two phenomenon that govern the conductivity in any device-

**Carrier concentration:**
- Electrons and holes are the charge carriers in a semiconductor. More is the number of carriers; greater is the conductivity of the material. Rise in temperature causes greater number of bonds to break due to higher number of collisions among vibrating molecules; thus, resulting in higher number of carriers with increase in temperature. This factor tends to increase the conductivity with increasing temperature. More the number of carriers, greater is the conductivity.

**Mobility of the carriers:**
- Mobility is another measure of conductivity. Greater is the mobility of carriers, carriers move with greater speed, thus, contributing more to the overall current; hence, greater is the conductivity of the material. With increase in temperature, lattice vibrations increase resulting in less mobility of free carriers. So, this factor tends to decrease the conductivity with temperature increase.

Summing up, the trend of conductivity with temperature depends upon which of the above two factors dominates. Based upon the conductivity, the materials can be divided into three types - conductors, insulators and semi-conductors. Let us explore how the conductivity of these materials is based on the above two factors:

**Conductivity of conductors (metals):** Metals have abundance of loosely attached nearly free electrons (as is commonly called, the electron sea), the carriers of electric current. The increase in carrier concentration is ignorable with change in temperature. So, mobility factor dominates. The conductivity of conductors decreases with increase in temperature.

**Conductivity of insulators (non-metals):** Insulators have almost negligible free carriers. The electrons in insulators are tightly bound to atoms by bonds. The conductivity is negligible in insulators due to limited number of carriers. However, the number of free carriers increases exponentially with temperature. This increase in carrier concentration with temperature outpaces the decrease in mobility thereby making the insulators to gain conductivity with rise in temperature. So, the conductivity in insulators increases with rise in temperature.

For CMOS transistors, the number of charge carriers directly translates to threshold voltage.

At high voltage levels applied, there is abundance of free charge carriers as a result of the energy supplied by the potential difference created. At this state, there is not significant change in carrier concentration with increase in temperature; so, the mobility factor dominates; thereby, decreasing the conductivity with temperature. In other words, at high levels of voltages applied, the conductivity of semiconductors decreases with temperature.

Similarly, in the absence of any voltage applied, or with little voltage applied, the semiconductor behaves similar to an insulator with very less number of carriers, those resulting from only thermal energy. So, increase in carrier concentration is the dominating factor. So, we can say that at low applied voltages, the conductivity of semiconductors increases with temperature.

**The concept of temperature inversion:** With reference to the discussion we had earlier, at higher technology nodes, the voltage levels used to be high. So, traditionally, the delay of CMOS logic circuits used to increase with temperature. So, the most timing critical corner used to be worst process, minimum voltage and maximum temperature. However, with scaling down of technology, the voltage levels have also scaled down. Due to this, at sub-nanometer technology levels, both the factors come into play. At lower range of the operating voltage levels, first factor comes into play. In other words, at lower technology nodes, the most setup timing critical corner has become worst process, minimum voltage and minimum temperature. This shift in setup critical corner, in VLSI jargon, is termed as temperature inversion.
