---
layout: post
title: Introduction to STA Part 1
---

## What is STA ?

Static timing analysis is one of the techniques used to verify the timing of a digital design.
STA is static since the analysis of the design is carried out statically and does not depend upon the data values being applied at the input pins of the design.

STA is a complete and exhaustive verification of all the timing checks of a design.

## Why we do STA ?

Timing analysis methods such as simulation can only verify the portions of the design that get exercised by simulus.

Verification through timing simulation is only as exhaustive as the test vectors used.

To simulate and verify all the timing conditions of a design with 10-100 million gates is very slow and timing cannot be verified completely.

So, STA on the other hand provides faster and simpler way of checking and analyzing all the timing paths in the design for any timing violations.

## Dynamic Timing Analysis (DTA) :

Dynamic Timing Analysis requires a comprehensive set of input vectors to check the timing paths in the design.

It determines the full behaviour of the circuit for a given set of input vectors.

Dynamic simulation can verify the functionality of the design as well as timing requirements.

## Difference between DTA and STA?

![figure1](http://www.signoffsemi.com/wp-content/uploads/2018/02/1.png)

## Input and Output files of STA

![figure2](http://www.signoffsemi.com/wp-content/uploads/2018/02/2.png)

**Setup Time** : The minimum time before the active edge of the clock, the input data must remain stable is called the setup time.

**Hold Time** : The minimum time after the active edge of the clock, the input data must remain stable is called the hold time.

![figure3](http://www.signoffsemi.com/wp-content/uploads/2018/02/8.png)
![figure4](http://www.signoffsemi.com/wp-content/uploads/2018/02/9.png)

## Setup timing checks :

The setup check ensures that the data is available at the input of the flip-flop before the active edge of the clock.

The data should be stable for a certain amount of time, namely the setup time of the flip-flop, before the active edge of the clock arrives at the flip-flops, so that the data is captured reliably into the flip-flop.

The setup check ensures that the data launched from the previous clock cycle is ready to be captured after one cycle.

![figure5](http://www.signoffsemi.com/wp-content/uploads/2018/02/10.png)
[Tlaunch + TCK2Q + Tpd] < [Tcapture + TCLK – TSetup]

## Hold timing checks:

The hold check ensures that the data is available at the input of the flip-flop after the active edge of the clock.

The data should be stable for a certain amount of time, namely the hold time of the flip-flop, after the active edge of the clock arrives at the flip-flops, so that the data is captured reliably into the flip-flop.

![figure6](http://www.signoffsemi.com/wp-content/uploads/2018/02/11.png)

[Tlaunch + TCK2Q + Tpd] > [Tcapture + Thold]

**Slew** - Slew is the transition time of a signal, the time it takes for a signal to change between two specific levels.

**Skew** : Skew is the difference in the arrival of the clock at two consecutive clock pins of sequential elements.

**Positive Skew** : If the capture clock comes late than launch clock then it is called positive skew.
**Negative Skew** : If the capture clock comes early than launch clock then it is called negative skew.

**Global Skew** : It is defined as the difference between the maximum insertion delay and the minimum insertion delay between two flip flops.

**Useful Skew** : If the clock is skewed intentionally to resolve the violations, it is called as useful skew.

**Clock Latency** : Clock latency is the time it takes from clock source to end point.

**Source Latency** : Source latency, also called as insertion delay, is the delay from clock source to the clock definition point.
Network Latency : Network latency is the delay from clock definition point to clock pin of the register (sink point).

## Clock Latency = [ ( Source Latency ) + ( Network Latency ) ]

![figure7](http://www.signoffsemi.com/wp-content/uploads/2018/02/16.png)

**Clock Uncertainty** : Clock uncertainty is the deviation of the actual arrival time of the clock edge with respect to ideal arrival time. The deviation happens mainly due to jitter and noise.

PrimeTime subtracts setup uncertainty value from the data required time when it checks setup time (maximum paths).

PrimeTime adds hold uncertainty value to the data required time when it checks the hold time (minimum paths).

**Jitter** : Clock jitter refers to the temporal variation of the clock period at a given point — that is, the clock period can reduce or expand on a cycle-by-cycle basis.

**False Path** : A false path is a path existing in a design which should not be analysed for timing. For example,

A path between two multiplexed blocks that are never enabled at the same time. In below diagram timing path starting from CLK pin of FF1 and ending at D pin of FF3 is a false path because it can’t exist in circuit operation because of inverted select signals at the multiplexer select pins.

![figure8](http://www.signoffsemi.com/wp-content/uploads/2018/02/18.png)

A path between flip-flops belonging to two clock domains that are asynchronous with respect to each other.

![figure9](http://www.signoffsemi.com/wp-content/uploads/2018/02/19.png)

**Halfcycle Path** : A path which requires only half cycle to capture the data. It is formed when data is launched on positive edge of the clock and captured on negative edge of the clock or when data gets launched on negative edge of the clock and gets captured on positive edge of the clock.

In such paths, setup check become more tight as setup gets only half cycle while hold constraint is relaxed by half cycle.

![figure10](http://www.signoffsemi.com/wp-content/uploads/2018/02/Screenshot-from-2018-02-28-19-48-56.png)

![figure11](http://www.signoffsemi.com/wp-content/uploads/2018/02/Screenshot-from-2018-02-28-19-48-56.png)

The hold check always occurs one cycle prior to the capture edge. Since the capture edge occurs at 10ns, the previous capture edge is at 0ns, and hence the hold gets checked at 0ns. This effectively adds a half-cycle margin for hold checking and thus results in a large positive slack on hold.

**Cross-talk Noise** : It is undesired change in the output values of victim due to switching in the input of aggressor. If one net is switching and other is at a constant value , the switching net may cause voltage spikes on other net. This is called as cross talk noise. Cross talk noise is evolving as a key source in degrading performance and reliability of high speed integrated circuits.

**Cross-talk Delay** : When there is some delay in output transition of victim due to input transition of aggressor, it is called as cross talk delay. It occurs when some transition is happening in both the nets. Cross talk delay depends on the switching direction of the aggressor and victim nets too. If input transitions occur in same direction then output transition of victim becomes faster and if input transitions occur in opposite directions then output transition of victim becomes slower and delay is more which may violate setup time.

## Virtual Clock :

- A virtual clock is a clock that exists but is not associated with any pin or port of the design.
- It is used as a reference in STA analysis to specify input and output delays relative to clock.
- A virtual clock can be defined with no specification of the source port or pin.
- Virtual clock are required to constraint the input port to register timing path and register to output port timing path.
- Advantage of virtual clock is we can specify the desired latency.

## What is the difference between clock buffer and normal buffer ?

- Clock buffer have equal rise time and fall time, therefore pulse width violation is avoided.
- Normal buffers may not have equal rise and fall time.
- To make equal rise and fall time in a clock buffer we make PMOS width nearly 2 – 2.5 times the width of NMOS. Hence it consumes more power.
- Clock buffers are usually designed such that an input signal with 50% duty cycle produces an output with 50% duty cycle.
