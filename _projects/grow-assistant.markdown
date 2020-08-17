---
layout: project
title:  Grow Assistant
subtitle: A homeassistant integrated smart grow light
image:  06.jpg
tags:   led grow light 3dprint nodered esphome homeassistant
hackaday: https://hackaday.io/project/171004-esp32-cam-smart-led-herbs-planter
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
| 1      |       | IKEA TILLGANG                       |      |


## 3D Design and printed parts

### 3D design animation overview

<iframe width="560" height="315" src="https://www.youtube.com/embed/3oLMcAhXuZg" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

### 3d printed components

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
| 1      |       | IKEA TILLGANG                       |      |

## Electrical connections and PCB solder

To solder the PCB can be a little bit tricky, so i made the following video as a guide:

<iframe width="560" height="315" src="https://www.youtube.com/embed/TXhWJUsacrM" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

## Homeassistant ESPHome config

<script src="https://gist.github.com/nkmakes/5cbb01c7a6e85998619a468e60582e12.js"></script>

## NodeRed Automation

![]({{ site.baseurl }}/images/node_Red.JPG)
