---
layout: post
title: Introduction to Mesh, H-Tree and Multi-Tap Clock
---

Usually with regular CTS, clbock tree is balanced primarily considering a slow corner. When timed in a fast corner, the delays of different cell sizes or cell types may scale differently to one another and differently to the RC delay of the connecting wires, leading to skew. This may lead to harder setup and/or hold timing closure.

### Insertion delay reduction using H-tree:
An H-tree is often able to use top routing layer resources shared with power distribution and an h-tree can exploit larger drivers to drive significant distances in these top routing layers. The combination of larger drivers and low RC routing layers reduces the non common path clock insertion delay, potentially increasing the performance.

Sometimes, uneven H-tree sink distributions may lead to additional delay in their path to ensure electrical symmetry. Electrical symmetry concept is used in designs containing placement blockages such as memories and macros.

### Mesh:
A further performance increase may be achieved by shorting the outputs of the H-tree together with a mesh. This has the advantage of reducing the non-common path clock insertion delay further but at the expense of additional power, routing resource and design work.

### Multi tap with H-tree:
An H-tree alone is not a complete solution. Regular CTS is required to complete the buffering and balancing required between the sinks of the H-tree and the clock sinks.

### Various types of clocking schemes
1) Traditional cts : Has cross corner skew, OCV, Very flexible for clock gating, handles blockages well and easy to use/implement useful skew.
2) Multi-Source cts:
3) Clock mesh: No cross corner skew, No OCV, inflexible and high power and routing resources.
4) Standard H-Tree
5) Flexible H-tree: No cross corner skew, some OCV, quite flexible.

### Standard H-tree advantages and disadvantages:
Advantages:
1) Very good cross corner scaling behavior.
2) Balanced by construction.
3) Lower power than mesh.

Disadvantages:
1) Need to have power-of-two number of sinks (tap buffers).
2) Need rectangular unblocked area.
3) Higher power than ad-hoc CTS tree.

How H-tree accommodates clock gate if the sink is given as clk port ?
