---
layout: project
title:  Homeassistant enabled smart grow light
image:  06.jpg
tags:   led grow light 3dprint
hackaday: https://hackaday.io/project/171004-esp32-cam-smart-led-herbs-planter
---
One day i was tired of buying fresh herbs, but unluckily i live in a apartment, where there is not direct sunlight most of the year.
Also i am a lazy guy and i wanted some kind of supervisor of the plant health and status, to track the plants, and automatize as much as possible, while its easy to setup
So i decided to create a tabletop planter which could give enough light to my plants, and also upload some stats and and images to a server.

Main features of first version:
- Smart home integrated (Homeassistant).
- custom PCB with only thru hole components (easy solder).
- Based on ESP32cam so plenty of implementation examples available.
- Fully 3d printed frame.
- High efficiency and dimmable LED Driver.
- 180 lm/W Led strips.
- Adjustable Height.
- OLED realtime status show.
- Easy OTA config with ESPHome, easy automation creation through nodered.
- I2C Pins exposed. 


## Components

| number | price | Component                           | Link |
| ------ | ----- | ----------------------------------- | ---- |
| 2      |       | BXEB L0280                          |      |
| 30     |       | M3 25mm aprox screws and nuts       |      |
| 2      |       | M2 / M2.5 25mm screws ands nuts     |      |
| 1      |       | Meanwell LDD-700L                   |      |
| 1      |       | 24V - 2A Constant voltage driver    |      |
| 1      |       | ESP32cam with 140º camera           |      |
| 1      |       | Wifi antenna                        |      |
| 1      |       | OLED screen                         |      |
| 1      |       | DHT22                               |      |
| 1      |       | MP1584                              |      |
| 2      |       | 18-24 AWG cable (RED/BLACK)         |      |


## 3D Design and printed parts



## Electrical connections and PCB solder



## Homeassistant integration with ESPHome


## NodeRed Automation
