---
layout: post
title: Low Power Design Strategies
---

Power consumption has become a very important factor in recent process nodes. In this article we will explore/discuss the increasing challenges of power consumption and various design strategies to reduce power consumption.

### Increasing Challenges of power
In earlier generations of IC design technologies, the main parameters of concern were timing and area. EDA tools were designed to maximize the speed while minimizing area. Power consumption was a lesser concern. CMOS was considered a low-power technology with fairly low power consumption at the relatively low clock frequencies used at the time, and with negligible leakage current.

*In recent years, however, device densities and clock frequencies have increased
dramatically in CMOS devices, thereby increasing the power consumption dramatically. At the same time, supply voltages and transistor threshold voltages have been lowered, causing leakage current to become a significant problem*.

High power consumption can result in high temperatures during operation. Higher operating temperatures also reduce reliability because of electromigration and
other heat-related failure mechanisms.

### Dynamic & Static Power
**Dynamic** power is consumed during the switching of transistors, so it depends on the clock frequency and switching activity.
**Static** power is the transistor leakage current that flows whenever power is applied to the device, so it is not related to the clock frequency or switching activity.

**Dynamic Power** is the energy consumed during logic transition on the nets, consisting of two components switching and internal power. Switching power results from charging and discharging of external load cap on the output of the cell. Internal power results from short circuit current that flows through PMOS-NMOS stack during a transition.

Switching power - The amount of energy dissipated in each transition depends on the supply voltage and the capacitive load of the net. Also, because the current flows only during logic transitions on the net, the long-term dynamic power consumption depends on the clock frequency (possible transitions per second) and the switching activity.

Internal power - is consumed during the short period of time when both PMOS and NMOS are ON. This condition results in large current to flow from VDD to VSS (short circuit current - crowbar current). *Lower threshold voltages and slower transitions result in more internal power consumption*.

![figure3](https://encrypted-tbn0.gstatic.com/images?q=tbn%3AANd9GcSGrL9dZIxUe9ulEWjMuRpEifRaWK8L5xYf-Q&usqp=CAU)

**Static(leakage) Power**
Leakage current was negligible in earlier CMOS technology. But, with shrinking device geometries and reduced threshold voltages(scaling) leakage power is becoming significant enough that we cannot neglect it anymore.

Main causes for leakage power are

1) reverse bias p-n junction diode Leakage - has always existed in CMOS designs. This is the leakage from n type drain to p-type substrate in NMOS transistor and from p-type drain to n-type substrate in PMOS transistor.

2) sub-threshold leakage - is the flow of current from source to drain when the transistor is held "off". However, with lower power supply voltages and lower threshold voltages, “off” gate voltages are getting close to “on” threshold voltages. Sub-threshold leakage current increases exponentially as the gate voltage approaches the threshold voltage.

In the past, the subthreshold conduction of transistors has usually been very small in the off state, as gate voltage could be significantly below threshold; but as voltages have been scaled down with transistor size, subthreshold conduction has become a bigger factor.

Subthreshold conduction or subthreshold leakage or subthreshold drain current is the current between the source and drain of a MOSFET when the transistor is in subthreshold region, or weak-inversion region, that is, for gate-to-source voltages below the threshold voltage.

The reason for a growing importance of subthreshold conduction is that the supply voltage has continually scaled down, both to reduce the dynamic power consumption of integrated circuits and to keep electric fields inside small devices low, to maintain device reliability.

The amount of subthreshold conduction is set by the threshold voltage, which sits between ground and the supply voltage, and so has to be reduced along with the supply voltage. That reduction means less gate voltage swing below threshold to turn the device off, and as subthreshold conduction varies exponentially with gate voltage

3) gate leakage - is the result of using an extremely thin insulating layer between the gate conductor and the MOS transistor channel. Gate oxides are becoming so thin that only a dozen or fewer layers of insulating atoms separate the gate from the source and drain. Under these conditions, quantum-effect tunneling of electrons through the gate oxide can occur, resulting in significant leakage from the gate to the source or drain.


![figure1](https://1.bp.blogspot.com/_Se0VANaI9uM/R9zNuwzfESI/AAAAAAAAAQU/djHncdmMb8k/s400/leakage+power+components+in+inverter.jpeg)


![figure2](https://ars.els-cdn.com/content/image/3-s2.0-B9780121709600500220-f20-12-9780121709600.jpg)


![figure4](https://qph.fs.quoracdn.net/main-qimg-c0f930c7448c405c4efec558408358cf.webp)

Leakage currents occur whenever power is applied to the transistor, irrespective of the clock speed or switching activity. Leakage cannot be reduced by slowing or stopping the clock. However, it can be reduced or eliminated by **lowering the supply voltage or by switching off the power to the transistors entirely.**
