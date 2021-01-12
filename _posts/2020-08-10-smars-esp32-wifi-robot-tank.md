---
layout: post
title:  "SMARS robot build guide, 3dprinted and modular tank based on esp32 and tb6112fng"
date:   2020-8-10 8:05:55 +0100
image:  /images/10.jpg
tags:   smars esp32 robot tank 3dprint
description: 3dprinted SMARS robot tank build guide and instructions with esp32, tb6612 and wifi async server hotspot control
---

## What is SMARS?
[SMARS or "Screwless Modular Assemblable Robotic System"][smars-link] It's a 3dprinted STEM educative robot. I like it over other alternatives because of:
- Screwless assembly, doesn´t need any screws or glue!
- Reduced number of components or  Very cheap and easy to find parts
- Modular - lot's of upgrades can be found on thingiverse!
- Using so small motors makes electronics easy to implement

## Why SMARS?
Wanted a small simple robot to test different control modules and after some search, i landed into [Smars modular robot project][smars-link].
This small robot is based around a 9V battery and a arduino uno board, i had laying around some esp32 based arduino uno sized board and a screw shield terminal so it looked like a perfect fit, i had already everything needed excepting some cheap 6V motors.

## 3d print and build
The model is very easy to print and build, no supports needed, had some problems when mounting the tracks but with a little bit of oil in the idle wheel everything works really well. In the parts of the original models uses 200rpm motors, mine are 300rpm as the other ones where not available, but its not any problem

Apart from the project stl's you will need a way to place the 18650 batteries on the frame, and some way to charge them.

To charge them the easiest way you will need two tp4056 modules, and make some charger, i used the following [Files](https://www.thingiverse.com/thing:2248981) and also to place the batteries i made the following [remix](https://www.thingiverse.com/thing:4720002). It's pretty simple but its snappable and you dont need to re-print any smars part.

## Electronics
### Parts List

- Esp32 Arduino UNO sized board
- Arduino UNO protoboard shield
- Tb6612FNG driver board
- 2x 18650 batteries
- 18650 Battery charger


### Schematics

For this project i made my own motor shield based upon the tb6612fng motor driver.

The board should have no problem with a 8V input from 2s 18650 directly for the motor driver. if you plan to use any other development board, check this point before.

![Smars robotic tank ep32 electronics]({{site.baseurl}}/images/smars_electronics.JPG)

Notice that all of the tb6612 are soldered to male pin headers to be able to configure the pins at will.

![Smars robotic tank ep32 electrnoic hat front]({{site.baseurl}}/images/smars_hat1.jpg)

![Smars robotic tank ep32 electronic hat back]({{site.baseurl}}/images/smars_hat2.jpg)

## SMARS esp32 Code

To make the logic part of the project, i used the Arduino framework for esp32 and some external libraries.
The board creates a WiFi hotspot, and a async server. In the html server, a joystick is displayed. The web browser calculates the movement depending on joystick coordinates and sends the data to a async server on the esp module. This server sends the speed settings to both motors drived by a tb6612fng driver board. You can find full project on the following [blog entry](https://nkmakes.github.io/2020/09/02/esp32-tank-robot-joystick-http-web-control/) or directly on [github](https://github.com/nkmakes/SMARS-esp32).


<script src="https://gist.github.com/nkmakes/d50d0627f8821a73645102e1be1dcb17.js"></script>



## Some action
<iframe width="560" height="315" src="https://www.youtube.com/embed/Z3jsNJ_2ksw" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
<iframe width="560" height="315" src="https://www.youtube.com/embed/uIImwilvI2s" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>





