---
layout: post
title:  Diy led grow light
date:   2020-07-05 15:01:35 +0100
image:  led_mw20w_front.jpg
tags:   led grow light 3dprint
---
This is a small 3d printed led light i made for my vegetables houseplant. 



Designed to be as cheap as possible and let you grow your own food,  but to give the maximum efficiency and don´t involve any soldering. It's perfect to raise not very light demanding plants or for early stages,  i'm sure it also can be used for photography, hydroponics, aquaponics,  any kind of indoor setup and anything else you got in mind! 

Uses bridgelux gen2 or gen3 L0280 led lights, and can use both 500k or 3500k cri80 variantes

As drivers there are potions for meanwell apc-35-1050 or lpc-60-1400 for a total output of 21W in the first case and 29W in the second.

Compared to commercially available solutions, has a lumen output  similar to a HLG 65 vegging board, efficiency close to DIY COB leds  (Vero, CXB3590, Citizen... ) but with a higher coverage area.

Full build should be under 30$ or 30€

## 3D Design and printed parts

All of design has been made using fusion360, if somebody needs any tweak i can share the source files.

You will need:

- 4x side stl

And depending on your driver choice:

- 1x apc or lpc stl
- 2x wago_apc or wago_lpc stl

![]({{ site.baseurl }}/images/render_apc.PNG)

![]({{ site.baseurl }}/images/render_lpc.PNG)

## Components

| number | price | Component                           | Link |
| ------ | ----- | ----------------------------------- | ---- |
| 4      |       | BXEB L0280                          |      |
| 30     |       | M3 25mm aprox screws and nuts       |      |
| 2      |       | M2 / M2.5 25mm screws ands nuts     |      |
| 1      |       | Meanwell APC-35-1050 or LPC-60-1400 |      |
| 1      |       | 220v/110v AC cable with plug        |      |
| 2      |       | 5way WAGO                           |      |
| 2      |       | cable connector                     |      |
| 2      |       | 18-24 AWG cable (RED/BLACK)         |      |



## Electrical connections

The electrical connections are pretty simple, all of the four stripes must go in parallel to have equal current.

For this you will need the 5 way wago connectors, where you connect both ends of DC voltage side of the driver and a cable from every strip.

AC drivers cables should be connected using connector of your coice to the AC cable with its corresponding plug.

![]({{ site.baseurl }}/images/led_mw20w_back.jpg)

## Other Links

You can find more detailed instructions on how to build this project on [instructables][instructables-link]

[instructables-link]: https://www.instructables.com/id/30-3D-Printed-Efficient-Led-Grow-Light/