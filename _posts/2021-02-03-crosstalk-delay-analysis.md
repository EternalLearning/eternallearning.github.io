---
layout: post
title: Crosstalk Delay Analysis
---

Assume there is a N1 net and has a rising transition at output and there is an aggressor and cc between these two nets.

The capacitive charge required from the driving cell in various scenarios can be diff as shown below:

## Case 1: Aggressor net steady
In this scenario, the driving cell for the net N1 provides the charge for cg and cc to be charged to vdd. The total charge provided by the driving cell of this net is thus (cg+cc) multiplies vdd.

## Case 2: Aggressor switching in same direction
In this scenario, the driving cell is aided by the aggressor switching in the same direction. If the aggressor transitions at the same time with the same slew, the total charge provided by the driving cell is only cg * vdd. If the slew of the aggressor net is faster than that of N1, the actual charge required can be even smaller than cg * vdd since aggressor net can provide charging current for cg also. Thus, required charge from driving cell with aggressor switching in same direction is smaller than corresponding charge for steady aggressor. cc is 0. Negative cross talk.

## Case 3: Aggressor switching in opposite direction
In this scenario, the cc is charged from -vdd to vdd. Thus, the charge on cc changes by (2 * cc * vdd). This additional charge is provided by both the driving cell of net N1 as well as aggressor net. cc is not 0. positive cross talk delay.

Four types of crosstalk delay contributions are computed for every cell and interconnect in the design.

1. positive rise delay
2. negative rise delay
3. positive fall delay
4. negative fall delay

The launch and capture clock edge are normally the same edge for hold analysis. The clock edge through the common clock portion cannot have different cross talk contributions for the launch clock path and capture clock path. Therefore, worst case hold analysis removes the crosstalk contribution from the common clock path unlike worst case setup analysis related to crosstalk contribution.

## Hierarchical design and analysis
For a large design, it is normally not practical to obtain parasitic extraction in one run. The parasitics for each hier block can be extracted sepearately. This in tunr require that a hier design methodology be used for design implementation. This implies that there be no coupling between signals inside the hier block and signal outside the block. This can be achieved either with no routing over the block or by adding shield layer over the block. In addition, signal nets shld not be routed close to the boundary of the block and any nets routed close to the boundary of the block shld be shielded. This avoids any coupling with the net from other blocks.

## Noise avoidance techniques
1. shielding : placing shield wires in same metal layer ensures that there is minimal coupling for critical signals. Since immediate layers (above and below) would normally be routed orthogonally, the cc across layers is minimized.
2. wire spacing
3. fast slew rate
4. maintain good stable supply: this is imp not for crosstalk but for minimizing jitter due to power supply variations. significant noise can be introduced on clock signals due to noise on power supply. adequate decoupling capacitances shld be added to minize noise on power supply. 
5. gaurd ring: in the substrate helps in shielding the critical analog circuitry from digital noise.
6. deep n well: same as above
7. isolating block: in hier flow, routing halos can be added to the boundary of blocks, isolation buffer can be added to each of IO of block.
