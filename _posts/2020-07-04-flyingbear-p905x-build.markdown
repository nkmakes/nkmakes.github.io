---
layout: post
title:  Flyingbear P905x build guide
date:   2020-07-05 15:01:35 +0100
image:  p905_main.jpg
tags:   3dprinter flyingbear p905x upgrades
---
This post is about the first 3d printer i got, the flyingbear P905X from aliexpress. At the time i have designed and tried some of different upgrades so i will list a how-to for anybody willing to repair or upgrade this printer.

## Print problems found



## Upgrades list

| Upgrade                   | price | pros and cons                                  |
| ------------------------- | ----- | ---------------------------------------------- |
| trianglelabs v6 hotend    | 79    | CTC prusa i3b                                  |
| Z axis anti-backlash nuts | 65    | Ratrig AM8 Frame Kit                           |
| Capricorn tube            | 5.37  | Linear bearings                                |
| Bottom electronics mount  | 22.80 | Bigtreetech SKR v1.3 mini                      |
| Octoprint                 | 12.41 | Heated bed                                     |
| LED Strip                 | 17.39 | Y cariage + bowden MK8 metal extruder          |
| Longer Z axis bearings    | 22    | Trieanglelabs v6 hotend+capricorn tube+nozzles |
|                           |       |                                                |
|                           |       |                                                |
|                           |       |                                                |
|                           |       |                                                |

### Aditional tools list

| number | price | Component                                  |
| ------ | ----- | ------------------------------------------ |
| 1      | 6.50  | meanwell LPV-60-12 (used by both printers) |
| 1      | 20    | Raspberry pi 3 model A+                    |
| 1      | 8.80  | Rpi 5mp cam                                |
| 1      | free  | 5050 Led strip                             |
| 1      | 0.70  | Mosfet module                              |
| 1      | 5     | Ikea Lack table                            |
| 1      | 1.50  | 12v to 5v buck converter                   |
| 2      | 3.50  | 120mm 12V Fans                             |
| 2      | 0.50  | Wago connectors                            |
| 1      |       | USB cable                                  |
|        | 43    | Total cost                                 |



## Electrical Wiring

The electrical connections are pretty simple, all of the four stripes must go in parallel to have equal current.

For this you will need the 5 way wago connectors, where you connect both ends of DC voltage side of the driver and a cable from every strip.

AC drivers cables should be connected using connector of your coice to the AC cable with its corresponding plug.

![](C:/Users/niko9/OneDrive/2020/spicy_coder/web/nkmakes.github.io/_posts/{{ site.baseurl }}/images/led_mw20w_back.jpg)

## Build Details

### 3d Printer

### Octoprint Server and table

## Photos of finished build

## End 

You can find more detailed instructions on how to build this project on [instructables][instructables-link]

[instructables-link]: https://www.instructables.com/id/30-3D-Printed-Efficient-Led-Grow-Light/