---
layout: post
title: What are data to data timing checks?
---
Firstly, let us answer the question what are data to data timing checks and why we need them.

Many a times, two or more signals at analog-digital interface or at the chip interface have some timing requirement with respect to each other. These requirements are generally in the form of minimum skew and maximum skew. Data checks come to rescue in such situations.

Data-to-data checks are setup and hold checks between two data pins (neither of these are defined as clocks). These are also referred to as non-sequential constraints. One data pin is the constrained pin, like a data pin of a flop, and the second pin is the related pin, which acts like a clock pin of a flop.

This check is useful in a design where it may be necessary to provide specific arrival times of one signal with respect to another.

Constraints can be defined in the timing library (.lib model) for a cell or can be set interactively using the set_data_check command in SDC.

**Example 1:** Non-sequential check defined in the timing library

pin ( D1 ) {

     direction : input;

     timing ( )

       related_pin : “D2";

       timing_type : non_seq_setup_rising;


       ...


     timing ( )

       related_pin : “D2";

       timing_type : non_seq_hold_falling;

**Example 2:** Data-to-data checks defined by SDC

set_data_check –rise_from D2 –to D1 –setup 1.0
set_data_check –fall_from D2 –to D1 –hold .5


There are two differences between non-sequential checks and data-to-data checks.

- In a non-sequential check, the setup and hold values are obtained from the timing library, where the setup and hold timing models are described in the library. In a data-to-data check, only a single value can be specified for the data-to-data setup or hold check. So, non-sequential checks in .lib is more accurate, because it is sensitive to the slew of constrained and related pins.
- A non-sequential check can only be applied to the pins of a cell, whereas a data-to-data check can be applied to any two arbitrary pins in a design.

Non-sequential constraints in .lib is more accurate because it is sensitive to the slew of constrained and related pins. However, if in case you specify data-to-data check through the library as well as with the set_data_check command interactively, then the value specified using set_data_check will be used for the data-to-data check.

The data to data setup check is performed on the same edge as the launch edge (unlike a normal setup check of a flip-flop, where the capture clock edge is normally one cycle away from the launch clock edge). That is why the data to data setup checks are also referred to as zero-cycle checks or same-cycle checks.

A non-sequential setup check specifies how early the constrained signal must arrive relative to the related pin.

A non-sequential hold check specifies how late the constrained signal must arrive relative to the related pin.
