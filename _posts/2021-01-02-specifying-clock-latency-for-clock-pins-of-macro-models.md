---
layout: post
title: How to specify clock latency for clock pins of macro models
---

## Problem statement
The clock input pins of macros (.lib model) typically need to be balanced earlier than other sinks because of internal clock paths inside the macro. How can the clock latency of a hard IP be specified for CTS?

## solution

A pin insertion delay represents the delay ‘underneath’ a clock sink as shown in following diagram:

![figure1](https://c.na14.content.force.com/servlet/servlet.ImageServer?id=015d000000BC9Ml&oid=00Dd0000000c1Z9&lastMod=1471882379000)

For a macro clock input pin, the pin insertion delay will represent the internal clock path delay inside the macro. This delay, at any clock pin of a macro model, can be specified using either of the following methods:

**1. Using the max_clock_tree_path/min_clock_tree_path attribute timing library**

CTS honors the clock latency defined in the library using the max_clock_tree_path / min_clock_tree_path attribute.

To enable CTS to consider max_clock_tree_path / min_clock_tree_path from .lib, you need to use below setting :

set_ccopt_property use_min_max_path_delays true

**2. Using the set_clock_latency SDC command**

If clock latency of the pin is represented by a pin-specific network latency using the set_clock_latency SDC command, this information is translated to the automatically-generated clock tree specification.

For example, the “set_clock_latency -1.222 [get_pins {mem1/CK}]” constraint is automatically translated to the positive 1.222  with set_ccopt_property insertion_delay command in the automatically-generated clock tree specification as shown below:

set_ccopt_property insertion_delay –pin mem1/CK 1.222

**3. Manually modifying the automatically-generated CCOpt clock tree specification**

If the set_clock_latency SDC command on the clock pin is not present, the automatically-generated clock tree specification can be manually modified to specify the insertion delay underneath the clock pin by using the following command:

set_ccopt_property insertion_delay –pin mem1/CK 1.222
