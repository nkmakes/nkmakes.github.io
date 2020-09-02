---
layout: post
title:  "Homeassistant (Hass.io) easy timelapse setup"
date:   2020-8-25 8:05:55 +0100
image:  /images/10.jpg
tags:   homeassistant homeautomation esp32cam raspberrypi
description: How-to guide to create your own cool timelapses from any Homeassistant camera entity the easy way
---


## Setting up Homeassistant
### Initial requeriments
Homeassistant
Working camera entity
Node-Red configured on hass.io server

### Setup folder permissions and installing needed modules

1. You need to create a folder named timelapses inside of your config folder. I use [file editor](https://github.com/home-assistant/hassio-addons/tree/master/configurator) for this.
You also need to add following line to your configuration.yaml file, under homeassistant: to grant permissions over the folder.

{% highlight yaml %}
homeassistant:
  #other config lines
  #...
  #...
  #end of other config lines
  whitelist_external_dirs:
  - /config/timelapses
{% endhighlight %}

Reload core config after this

2. Now you should check if homeassistant is able to take a photo. For this go to your "Services" tab under "Supervisor" and try to run a camera.snapshot service over your camera, in my case camera.esp32_cam. Copy the following YAML entry acording to your entity name as shown in the photo. Call the service with big blue button.


{% highlight yaml %}
entity_id: camera.esp32_cam
filename: '/config/timelapses/test_photo.jpg'
{% endhighlight %}


![Hass homeassistant timelapse photo check]({{site.baseurl}}/images/hass_service_test.png)

3. Install fs-ops-node red module
You need to open your node-red webUI and under "Options", under "Manage Pallete" tab you need to install the [following module](https://flows.nodered.org/node/node-red-contrib-fs-ops)
![Hass homeassistant node-red-module]({{site.baseurl}}/images/node_red_fs_install.jpg)

4. Install moment node red module (if not present)
Based on a solution from this [post](https://discourse.nodered.org/t/msg-filename-logging-data/1117/13)

5. Install Homeassistant Samba module (if not already) and configure to access from win 10
Following [guide]() came in pretty handy.

## Configuring your timelapse

We are going to make all the shots at desired timeframe, for this we will use node red, if you want to import the flow, you can find it [here]()
I used a 6 min interval, so i can have each 24h in a 20s video at 24fps.

### General flow schematics

![Node-red timelapse general schematics]({{site.baseurl}}/images/timelapse_node.jpg)

### Retrieve the photos

At this moment i couldnt find any easy way to make the videos over my hassOS install.
So to create the timelapse we will need to download the images using a samba server. 
We can access the photos of each day at your configured location, in my case /config/timelapses

### Creating the timelapse

For this by the moment i'm using photoshop, as i'm retrieving the photos under a windows 10 OS, but there a re plenty of other solutions available


[smars-link]: https://www.thingiverse.com/thing:2662828


