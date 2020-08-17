---
layout: post
title:  "MR6 esp32cam robot tank"
date:   2020-8-9 8:05:55 +0100
image:  02.jpg
tags:   esp32cam robot tank project
---

With the aim of creating a cheap 3d printed tank, i found timmiclark's awesome [MR6 tank model][mr6-link] and decided to make mine using his design, and create all the electronics and program based on the cheapest wifi and camera capable based module, the Ai-thinker esp32cam, and tb6612fng driver.



The idea is to have a battery powered tank with enought WiFi range that could hot a async https web server with a joystick and the webcam image streaming.

## 3d print and build
I got several problems during the print of the model, being my 3dprinters not enough accurate causing the mechanics components couldn't be assebled, so i modified some of the pieces to have a bigger hole etc on tinkercad

## Electronics
The electronics is based on a custom PCB blank board with a custom circuit implemented around the esp32cam and the driver. Offers external 12V VIN input and motor output screw terminals.

The motor driver uses all of the available pins result of not using the microsd module already incorporated on the module. 

## Code

The module firmware is very similar to SMARS robot one but adding the camera stream capabilities.


<script src="https://gist.github.com/nkmakes/d50d0627f8821a73645102e1be1dcb17.js"></script>



## Some action
<iframe width="560" height="315" src="https://www.youtube.com/embed/Z3jsNJ_2ksw" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
<iframe width="560" height="315" src="https://www.youtube.com/embed/uIImwilvI2s" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>


[m66-link]: https://www.thingiverse.com/thing:2753227


