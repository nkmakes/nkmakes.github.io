---
layout: post
title:  "MR6 esp32cam robot tank"
date:   2020-8-9 8:05:55 +0100
image:  02.jpg
tags:   esp32cam robot tank project
---

Wanted a small simple robot to test different control modules and after some search, i landed into [Smars modular robot project][smars-link].
This small robot is based around a 9V battery and a arduino uno board, i had laying around some esp32 based arduino uno sized board and a screw shield terminal so it looked like a perfect fit, i had already everything needed excepting some cheap 6V motors.

## 3d print and build
The model is very easy to print and build, no supports needed, had some problems when mounting the tracks but with a little bit of oil in the idle wheel everything works really well. In the parts of the original models uses 200rpm motors, mine are 300rpm as the other ones where not available, but its not any problem

## Electronics
As mentioned i used a esp32 arduino sized board and mounted over it a little homemade shield with a tb6612 driver and external VIN

## Code


<script src="https://gist.github.com/nkmakes/d50d0627f8821a73645102e1be1dcb17.js"></script>



## Some action
<iframe width="560" height="315" src="https://www.youtube.com/embed/Z3jsNJ_2ksw" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
<iframe width="560" height="315" src="https://www.youtube.com/embed/uIImwilvI2s" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>


[smars-link]: https://www.thingiverse.com/thing:2662828


