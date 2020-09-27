---
layout: project
title:  3dprinted neopixel ring pitfire
subtitle: Fully 3dprinteable pitfire, based upon 16 ws1282b LED ring and esp32 with wled server. 
image:  /images/led_mw20w_front.jpg
tags:   neopixel esp32 homeassistant iot
description: Bridgelux L0280 based 3d printed light fixture, under 30$ in materials and 180 lm/w efficiency.
instructables: https://www.instructables.com/id/30-3D-Printed-Efficient-Led-Grow-Light/
hackaday: https://hackaday.io/project/170979-best-diy-led-grow-light-fully-3dprinted-for-30
thingiverse: https://www.thingiverse.com/thing:4094951
myminifactory: https://www.myminifactory.com/es/object/3d-print-30-very-efficient-led-grow-light-117785
---



## 3D Design and printed parts

This object is based around two 3d printed parts, a flame, designed to be 3d printed in vase mode, preferently in white or transparent plastic, and a base that fits the neopixel ring and the esp32 dev board.

For the base i provide two stl's, a simple base withouth any magic or a "stone pitfire" look, but you have unlimited options, you can find in the github repo of this project, original fusion 360 files so you can add any base that you could imagine, or if you are not familiar with the program, you can also edit the blank stl base file with Tinkercad, which is way easier for beginners.

For the base i used 0.4 mm nozzle 0.3 mm height with 2 bottom and top layers and 2 permiter lines with 10% infill and works ok, for the flame you should print it with vase print, no botom layer, and some with some brim to fix it to the hotplate.
Both prints should be under 80gr of plastic.


## Components

| number | Component                           |
| ------ | ----------------------------------- |
| 1      | 16 led Neopixel ring                |
| 1      | esp32 30 pin dev board              |
| 3      | broken female dupont wires(or two female-female)|
| 1      | 2A microUSB cable                   |
| 1      | 5V 2A USB power source              |
| 80 gr  | PLA filament                        |



## Electrical connections

Electric connections on this project are very simple.
First, you need to cut three dupont female ends with some 6 cm of cable each, and solder them to the three pads you can find on the underside of the Neopixel Ring labelled as 5V, GND and DI.

They go connected the next way:
| esp32 | Neopixel |
| ----- | -------- |
|VIN|5V|
|GND|GND|
|D2|DI|

## ESP32 WLED install and config

For the control of the leds we are going to use [WLED program](), with this we are going to be able to control our LEDs from any web browser as an AP, or we can can also add it to our local network, and it's integrable to most of the smart home applications as HASS or OpenHAB.

You can refeer to original [WLED install guide]() for updated install steps, but in brief, at time of writing this gudie, we need to:
1. Download latest [ESP flashing tool]().
2. Download latest [ESP32 firmware]().
3. Flash the firmware.
At this point, we should be able to connect to the board and conect to the AP on 1.2.3.4 using the passw wled4321.
4. If we want to control it from the local network, add local net credentials.
5. If we want to add it to HASS, it should automatically appear as a device.

## Final assembly

To assembly the device, we need to first connect the three cables to the ESP32 dev board, then place it insedie of the circle, connect the cable and put the board in the final position. If we want to fix the Neopixel ring in place you can use a bit of liquid adhesive.

## Final Result
