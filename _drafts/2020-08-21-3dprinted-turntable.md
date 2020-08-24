---
layout: post
title:  "3d printed turntable"
date:   2020-8-10 8:05:55 +0100
image:  /images/10.jpg
tags:   3dprint esp8266 esp32
description: Guide to build your own esp8266 wifi controlled turntable, based upon thingiverse user Bribro12 Fully 3D-printable turntable design.
---
I wanted a turntable to show off some prints, and after looking all-around the web i falled into amazings Bribro12's [Fully 3d-printable turntable](https://www.thingiverse.com/thing:3723618) and [Arduino controlled photogrammetry 3D-scanner](https://www.thingiverse.com/thing:3958326) designs. 

I liked a lot the gear/bearing system but i wanted to change a bit the aesthetics and electronic components, so i decided to make the "encolsure" or "base" of the design to fit my needs.


## Required components

esp8266
12V to 5v Buck converter
28-byj
uln2003
12V power source
Dupont cables

## 3dprinted components

### From [original design]()
1 Outer Gear
1 Inner Gear

### From my [mod files]()
1 BasePlate
1 Top platform from your election

## Build Guide

### Electronics connections

On following scheme you can find detailed required connections. 
In brief:

    - You need a power supply. You can use or a 12V power source or any 5V usb Converter with more than 1A.
    - In case you using 12V power supply design. You need to use a buck converter.
    - You need to connect both power supply cables (+) & (-) to esp8266 board and motor driver.
    - You need to add dupont cables from the ULN2003 stepper motor driver to the esp8266 board pins.
    - Connect the motor to the motor driver.

Schematic:


### Assembly


