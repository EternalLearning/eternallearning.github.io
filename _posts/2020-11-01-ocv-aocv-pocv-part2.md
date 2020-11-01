---
layout: post
title: OCV, AOCV, POCV Part 2
---

One of the primary challenges is variation in manufacturing parameters, namely random and systematic variations. To model these parameter variations, a few engineers came up with the on-chip variation (OCV) model. The concept of OCV was first introduced in technology nodes above 90nm. The fundamental idea behind OCV is to apply global derates on the whole design irrespective of the type of cells, its individual variation or its slew-load conditions. But this simple concept became ineffective in lower technology nodes. Unfortunately, global derates make the design too optimistic for shorter paths and too pessimistic for longer paths. Subsequently, expected results are not accurate and reliable enough, which affects the performance of the chip.

In order to overcome the extra pessimism added due to OCV, advanced on-chip variation (AOCV) technique was introduced for nodes below 65nm. AOCV technique adds derates in the design based on the logic depth and distance of the cell in the timing path. The depth of the cell models the random variation component, while distance (location) of the cell in the path models systematic variation of the cells. But below 40nm, this method too becomes inaccurate as it cannot further reduce pessimism. To address the shortcomings of AOCV, parametric on-chip variation (POCV) evolved.

In POCV, instead of applying a specific derating factor to a cell, cell delay is calculated based on a delay variation of that cell. This delay variation (σ ) for each cell is obtained through Monte-Carlo HSPICE simulation. The variation value σ is a unique value specific to that library cell.

## Some of the terminologies used for POCV analysis are briefly explained below:

Normal distribution curve : Data distribution for any entity can be spread out in different ways. [4] In a normal distribution curve, the data tends to be around a central value with no bias on the left or right, as shown in Figure 1 .

![figure1](https://www.edn.com/wp-content/uploads/contenteetimes-images-01rocketman-ednpocvf1x600.jpg)

For POCV, it is assumed that the nominal delay (expected delay) value of a cell follows a normal distribution curve. The delay value of the cell is calculated after many experiments and their mean is taken as the nominal delay.

Standard deviation (σ ): For a normal distribution curve, data tends to be denser in the center and less dense on the edges as shown in Figure 1. In order to measure the variations from the mean, the metric named standard deviation (σ) is used. It measures how far any data value deviates from the mean. If the delay values of the cell deviate from the nominal value to the left or right by any amount, then more changes in the timing of that cell are expected. This deviation of the delay from its expected value is modeled using σ, as shown in Figure 2 .

![figure2](https://www.edn.com/wp-content/uploads/contenteetimes-images-01rocketman-ednpocvf2x600.jpg)

The normal distribution curve obeys the following rules [5] :

- About 68% of the area under the curve falls within 1σ of the mean.
- About 95% of the area under the curve falls within 2σof the mean.
- About 99.7% of the area under the curve falls within 3σ of the mean.

## POCV analysis

POCV uses a nominal value for modeling random variations on the die instead of using min-max delay values for timing arc. When using min-max values to model delay variations, there is a probability of actual delay falling anywhere between the min-max extremes. While in POCV, there are maximum chances of the delay occurring near the nominal value and fewer chances of falling farther away from it. POCV calculates the arrival time and required time statistically instead of using the min-max values. Hence, the results are less pessimistic and more accurate.

POCV analysis uses nominal delay (μ) and variation (σ) for timing analysis in the following way:

1. The tool takes the value of σ from the timing library or an external file containing POCV coefficient C.

2. Each arc timing is then calculated statistically as the total of the nominal delay and the variation.

3. The tool then calculates the delay of the path by statistically combining these arc delays and then checks whether the chip works at the expected frequency.

By default, the tool performs POCV analysis at three standard deviations (3σ ) from the mean. One can specify the number of standard deviations to be used for delay calculation. Increasing the value of standard deviations tightens the timing requirement making it more pessimistic.

![figure3](https://www.edn.com/wp-content/uploads/contenteetimes-images-01rocketman-ednpocvt1ax600.jpg)
