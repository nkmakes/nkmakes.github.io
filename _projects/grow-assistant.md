---
layout: project
title:  Grow Assistant
subtitle: A homeassistant integrated smart grow light
image:  /images/06.jpg
tags:   led grow light 3dprint nodered esphome homeassistant
description: A Hass.io (homeassistant) enabled 3d printed grow light. Based on esp32 cam with a custom made PCB.
hackaday: https://hackaday.io/project/171004-esp32-cam-smart-led-herbs-planter
thingiverse: https://www.thingiverse.com/thing:4574145
github: https://github.com/nkmakes/growassistant/
---

One day i was tired of buying fresh herbs, but unluckily i live in a apartment, where there is not direct sunlight most of the year.
Also i am a lazy guy and i wanted some kind of supervisor of the plant health and status, to track the plants, and automatize as much as possible, while its easy to setup
So i decided to create a tabletop planter which could give enough light to my plants, and also upload some stats and and images to a homeassistant server.

## Main features of first version:



- Smart home integrated (Homeassistant), easy OTA config with ESPHome, Automation throught nodeRED.

- Custom PCB with only thru hole components (easy solder).

- Based on ESP32cam so plenty of projects documentation available.

- Fully 3d printed frame.

- High efficiency and dimmable LED Driver and strips.

- Different light spectrum available.

- Adjustable Height.

- OLED realtime status show.

- Easily hackable (I2C Pins exposed).

  

<iframe width="560" height="315" src="https://www.youtube.com/embed/djoQBKrrGDU" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

---

## Components

| number | Component                        |
| ------ | -------------------------------- |
| 2      | BXEB L0280                       |
| 30     | M3 25mm aprox screws and nuts    |
| 2      | M2 / M2.5 25mm screws ands nuts  |
| 1      | Meanwell LDD-700L                |
| 1      | 24V - 2A Constant voltage driver |
| 1      | ESP32cam with 140º camera        |
| 1      | Wifi antenna                     |
| 1      | OLED screen                      |
| 1      | DHT22                            |
| 1      | MP1584                           |
| 2      | 18-24 AWG cable (RED/BLACK)      |
| 1      | IKEA TILLGANG                    |
| 6      | 11x11x11 1L plastic pots         |

---

### BOM

| Part           | Device                  | Package                | Description                                                  |
| -------------- | ----------------------- | ---------------------- | ------------------------------------------------------------ |
| BOOT           | PINHD-1X2               | 1X02                   | PIN HEADER                                                   |
| DHT22          | PINHD-1X3               | 1X03                   | PIN HEADER                                                   |
| ESP32-CAM      | ESP32-CAM               | ESP32CAM               | Modules PCBA Module RoHS                                     |
| FTDI           | PINHD-1X4               | 1X04                   | PIN HEADER                                                   |
| I2C_1          | PINHD-1X4               | 1X04                   | PIN HEADER                                                   |
| I2C_2          | PINHD-1X4               | 1X04                   | PIN HEADER                                                   |
| J1             | DCJ0303                 | DCJ0303                | DC POWER JACK                                                |
| J2             | CONN_022.54MM_SCREWTERM | 1X02_2.54_SCREWTERM    | Multi connection point. Often used as Generic Header-pin footprint for 0.1 inch spaced/style header connections |
| J3             | CONN_022.54MM_SCREWTERM | 1X02_2.54_SCREWTERM    | Multi connection point. Often used as Generic Header-pin footprint for 0.1 inch spaced/style header connections |
| LDD-700L       | LDD-700L                | LDD-XXXL               | LED Power Supplies 9-36Vin 2-32Vout 300mA LED Driver         |
| MP1584-24VTO5V | DC-DC-STEP-DOWN-MP1584  | DC-DC-STEP-DOWN-MP1584 | DC/DC Step-Down Regulator based on MP1584 chip               |

---

## 3D Design and printed parts

### 3D design animation overview

<iframe width="560" height="315" src="https://www.youtube.com/embed/3oLMcAhXuZg" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

---

### 3d printed components

![grass esp32cam lamp full render]({{ site.baseurl }}/images/esp32cam_lamp_full_render.png)

| number | Component                        |
| ------ | -------------------------------- |
| 1      | grass_top_lamp_part.stl (Yellow) |
| 2      | grass_reflector_body.stl (White) |
| 1      | grass_bottom_base.stl (Red)      |
| 2      | grass_bottom_stick.stl (Green)   |





---

## Electrical connections and PCB solder

Grass has its own small PCB board that puts together all the electronic components. Here you can check out the schematics and layout

![grass pcb schematic]({{ site.baseurl }}/images/schema.JPG)

![grass pcb]({{ site.baseurl }}/images/grass_pcb.JPG)

![grass pcb render]({{ site.baseurl }}/images/pcb_render.JPG)

Some board render images:

![grass esp32 pcb board mounted render]({{ site.baseurl }}/images/esp32_cam_2020-Apr-07_11-49-43AM-000_CustomizedView3758424710.png)

![grass esp32 pcb board mounted render detail]({{ site.baseurl }}/images/esp32_cam_2020-Apr-07_11-36-41AM-000_CustomizedView25932347027.png)

To solder the PCB can be a little bit tricky, so i made the following video as a guide:

<iframe width="560" height="315" src="https://www.youtube.com/embed/TXhWJUsacrM" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>



---

## Homeassistant ESPHome configuration

To control the light you need to add the ESPHome extension on the RPi and install a firmware on the esp32.

The following sketch contains the directives for ESPHome:

<script src="https://gist.github.com/nkmakes/5cbb01c7a6e85998619a468e60582e12.js"></script>



## NodeRed Automation

I used a nodered core server on the raspberry pi to control the the esp32cam, with esphome is pretty forward, in my case i am using the Sun component to turn on lights at sunrise and turn them down at dawn, but any time of automation is posssible.

![grass nodered sketch]({{ site.baseurl }}/images/node_Red.JPG)