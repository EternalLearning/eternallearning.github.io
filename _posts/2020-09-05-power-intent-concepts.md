---
layout: post
title: Power Intent Concepts
---

In the UPF language, a power domain is a group of elements in the design that share a common set of power supply needs.

Each power domain has a scope and an extent. The *scope* is the level of logic hierarchy designated as the root of the domain. The *extent* is the set of logic elements that belong to the power domain and share the same power supply needs. The scope is the hierarchical level at which the domain is defined and is an ancestor of the elements belonging to the power domain, whereas the extent is the actual set of elements belonging to the power domain.

Basic Power Commands:

create_power_domain
create_supply_net
create_supply_port
connect_supply_net
create_power_switch (power flow direction)
set_level_shifter
set_isolation - for inserting isolation cells at the outputs of switched (power-down) domains.
set_isolation_control
set_always_on_strategy
set_retention - to insert retention cells inside switched(power-down) domain.
create_pst - power state table defines legal combinations of states that can exist at the same time during operation of a design.  The state of a power domain consists of the set of on-off states and voltage levels of its supply ports. The power state table is used for synthesis, analysis, and optimization.

The size_only attribute is set on all inserted isolation cells, level shifters, and enable registers to prevent them from being optimized away. This ensures that thae isolation and level -shifting functions between power domains are maintained throughout the flow.

While there are several level shifter implementations, they can be divided into two major classes: simple buffer-type level shifters and enable-type level shifters (often called enable level shifters). An enable level shifter, in addition to shifting the voltage level between input and output, can maintain its output fixed at a steady value that is controlled by the enable signal, which allows the shut-off of its input pin. Design Compiler automatically inserts buffer-type level shifters in a compile operation

Level shifters should also be added to the clock tree nets. The clock tree synthesis engine
in IC Compiler assumes that all required level shifters have been added beforehand.

If level shifters are missing on particular nets, the timing engine maintains accurate timing
analysis by automatically scaling transition times and delays according to the voltage on
either side of the voltage interface.

Design Compiler inserts isolation cells where signals leave a power-down domain. After an isolation cell is inserted, a dont_touch attribute is automatically set on the net segment connecting the isolation cell power domain interface. This protection helps prevent optimizations from placing cells on that piece of net.

Compile automatically inserts buffer-type level shifters on the appropriate nets and purges
all redundant level shifters. By default, it does not insert level shifters on clock nets.
