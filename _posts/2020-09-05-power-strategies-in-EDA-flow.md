---
layout: post
title: Handling Power in EDA Flow
---
DC low power flow: The size_only attribute is set on all inserted isolation cells, level shifters, and enable registers to prevent them from being optimized away. This ensures that thae isolation and level -shifting functions between power domains are maintained throughout the flow.

While there are several level shifter implementations, they can be divided into two major classes: simple buffer-type level shifters and enable-type level shifters (often called enable level shifters). An enable level shifter, in addition to shifting the voltage level between input and output, can maintain its output fixed at a steady value that is controlled by the enable signal, which allows the shut-off of its input pin. Design Compiler automatically inserts buffer-type level shifters in a compile operation

Level shifters should also be added to the clock tree nets. The clock tree synthesis engine
in IC Compiler assumes that all required level shifters have been added beforehand.

If level shifters are missing on particular nets, the timing engine maintains accurate timing
analysis by automatically scaling transition times and delays according to the voltage on
either side of the voltage interface.

Design Compiler inserts isolation cells where signals leave a power-down domain. After an isolation cell is inserted, a dont_touch attribute is automatically set on the net segment connecting the isolation cell power domain interface. This protection helps prevent optimizations from placing cells on that piece of net.

Compile automatically inserts buffer-type level shifters on the appropriate nets and purges
all redundant level shifters. By default, it does not insert level shifters on clock nets.

**Leakage Power Optimization**
Leakage power optimization using multi-threshold libraries results in leakage power savings.
The set_leakage_optimization true command enables leakage optimization during compile. When your libraries support multiple threshold voltages for the cells, you can also use the set_multi_vth_constraint command to set the multithreshold voltage constraint. This command also allows you to specify whether this constraint should have higher or lower priority than the timing constraint.

The recommended methodology is to use all multiple-Vt libraries for logic synthesis in Design Compiler, and also for physical optimization in IC Compiler. The tool optimizes the design while providing equal emphasis for timing and power and making sure that the number of low-Vt cells used is kept to a minimum. This methodology gives comparable power numbers to that of two-pass leakage flow while maintaining good timing quality.

Use all multi-Vth libraries
set target_library "lvtwc.db svtwc.db_hvtwc.db"
set_multi_vth_constraint -lvth_groups {lvt svt} \
 -lvth_percentage 15 -type soft -cost cell -include_blackboxes

Enable leakage optimization
set_leakage_optimization true

Run compile
compile_ultra -scan

## Scan methodology using DFT compiler
If a scan chain crosses power domain boundaries, additional level shifters and isolation cells
are required on the scan path to take care of voltage shifting and isolation requirements. This impacts design timing and area quality of results. Therefore, the most common requirement for multivoltage-aware DFT methodology is to make sure scan insertion is bounded by the power domains. In addition, when two or more power domains share the same scan chain, the number of domain crossing points should be minimized.

When you run DFT Compiler in Design Compiler, any necessary multivoltage special cells (level shifters and isolation cells) are automatically inserted according to the strategies specified by UPF commands. The insertion strategy specifies the types of level shifters or isolation cells that are needed when a scan path crosses a power domain boundary and the locations where these cells are inserted.

By default, DFT Compiler clusters the scan chains within a power domain. This clustering
reduces the number of power domain crossings and therefore the number of level shifters
and isolation cells needed in the scan path.


## ICC methodology in handling UPF
When implementing multivoltage designs, these objects are required to create voltage areas, to derive interface net constraints, and to perform specific optimizations.

Always-on synthesis might require specific spots in the floorplan where the supply is always
active. Always-on cells can be placed only on those spots that do not require any interface
cells.

The floorplan is the first design stage where the voltage area can be defined. If you cannot
identify the best location for a voltage area, you can run a quick placement and trace the voltage-area-related logic to determine a suitable location. On the other hand, if you know an ideal location for a voltage area, you can specify its coordinates explicitly.

derive_pg_connection - command makes incremental power connections automatically for any new cells added during optimization. In recent ICC version releases, this command is performed implicitly by the place_opt, clock_opt, and route_opt commands.

Power-related cells such as level shifters, isolation cells, retention registers, always-on cells, and power switches typically have multiple power and ground pins and are taller than
standard cells. As a designer, you need to make sure that all the power pins are connected
and correctly routed to the power nets.

For example, consider the low-to-high level shifter cell with two power pins. The cell is taller than a standard cell and has 2X height. The secondary power pin, VDDL, should be connected to the power supply net VDD2. For power supply routing, VDDL needs to be routed to the VDD2 strap. However, this cannot be achieved with the normal signal or power routing method. A special type of routing, called net mode routing (specified with the command preroute_standard_cells –mode net), is required to connect the secondary pin to the nearby power strap.


On the other hand, level shifter or isolation cells might have more complex structures. A
possible way to accommodate these special cells in the floorplan is to place them by
themselves in a dedicated area. However, degraded quality of results can be expected due
to the imposed placement restriction.


Power switch cells require special placement near the power straps. The create_power_switch_array command is used to insert and place the switch cells automatically on the specified insertion grid. After placing the switch cells, the sleep net, which connects all the sleep pins of switch cells, is connected by using the connect_power_switch command.

When multicorner-multimode technology is used, optimization works on the worst timing violations across all scenarios, thus eliminating the convergence problems observed in sequential approaches. Optimization can be performed for DRC, setup and hold, or setup only.
