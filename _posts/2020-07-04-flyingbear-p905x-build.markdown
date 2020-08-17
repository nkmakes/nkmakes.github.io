---
layout: post
title:  Flyingbear P905x build guide
date:   2020-07-05 15:01:35 +0100
image:  /images/p905_main.jpg
tags:   3dprinter flyingbear p905x upgrades
---
This post is about the first 3d printer i got, the flyingbear P905X from aliexpress. At the time i have designed and tried some of different upgrades so i will list a how-to for anybody willing to repair or upgrade this printer.

## Print problems found

- 

## Recommended upgrades list

### Hotend
- trianglelabs v6 hotend ([files][]): Original hotend jams easily and is more difficult to find replacements, and is more difficult to calibrate in retractions.
- Nozzle fan ([files][]): This print lets you 
- Capricorn tube: If you let it with a default bowden extruder, this printer needs a pretty long tube, a smaller inner diameter tube will take away most of your retraction problems.

### Frame and electronics:
- Glass for the heatbed: I usually use some glass from old scanner and make it cut to the perfect size, 3mm glass is the best.
- Z axis mechanical endstop([files][]): Original magnetic endstop never worked well for me and with the glass dont worked at all, so this is just mandatory in some cases.
- tmc2209 drivers: Let's you implement sensorless homing and makes the printer way more quiet, i also changed the board for a SKR v1.3 as the original MKS board started to give problems.
- Z axis screws support ([files][]): Centers the z axis threaded rods in the middle smoothening the z axis movement.
- Under bed electronics case with 120mm fan ([files][]): After working on different cases on the back side of the printer, i decided to remake everything for a bottom based electronics case, it makes the cable management easier and makes the printer look way more neat, the con is that you will need to make some of the cables longer. Fiths original MKS GEN L board but also SKR V1.3.


## Notes during assembly

### Aditional assembly tools


## 