---
layout: post
title: Halo vs blockage
---

Blocakges are specific locations where placing of cells are prevented or blocked. These act as guidelines for placing std cells in the design. Blockages are of following types:

Soft blockage: Specifies a region where only buffers can be placed.

Hard blockage: Specifies a region where all std cells and buffers cannot be placed. Usually used to avoid routing congestion at macro corners, block std cells to certain regions in the design.

Partial blockage: The blockage factor for any blockage is 100% by default. So no cells can be placed in that region, but the flexibility of blockages can be chosen by partial blockages.

placement blockage: prevent the placement tool from placing cells at specific regions. Placement blockages are created at floor planning stage.

placement blockages are used to:

- Define std cells and macro area.
- Reserve channels for buffer insertion.
- Prevent cells from being placed nearer to macros.
- Prevent congestion near macros.

Routing blockage: Routing blockages block routing resources on one or more layers. It can be created at any point in the design.


Halo (Keep -out region):

- Halo is the region around the boundary of fixed macro in the design in which no other macro or std cells can be placed. Halo allows placement of buffers and inverters in its area.
- Halos of two adjacent macros can be overlap.
- If the macros are moved from one place to another place, Halos will also be moved. But in the case of blockages if the macros are moved from one place to another place the blockages cannot be removed.
