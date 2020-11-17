---
layout: post
title: Interface timing miscorrelation block vs top level
---

## IO Timing Miscorrelation between Block Level and Top-Level

There are a couple of reasons, which lead to miscorrelation between block-level and top level IO timing:

1. Inappropriate modelling of the driver pin and output load for the IO pins.
2. Missing constraints.
3. Different StarRc and signoff tool recipe or version used at the block level and top-level.
4. Mismatch in de-rating numbers in the block level and top-level runs.
5. cross talk on clocks
6. if budgetting is done using ideal clock based.
7. If uncertainity numbers are different in block and top level.
