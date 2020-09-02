---
layout: post
title:  "Homeassistant (Hass.io) easy timelapse setup"
date:   2020-09-01 8:05:55 +0100
image:  /images/timelapse_node.JPG
tags:   homeassistant homeautomation esp32cam raspberrypi
description: How-to guide to create your own timelapses from any Homeassistant camera entity the easy way, in this example, a esp32cam gonfigured with ESPHome.
---

After some time searching, i didn´t find a easy guide to make something as easy as getting a timelapse from a esp32cam on homeassistant, you can also can with the following guide.

## Setting up Homeassistant
### Initial requeriments
- Homeassistant
- Working camera entity
- Node-Red configured on hass.io server

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
![Hass homeassistant node-red-module]({{site.baseurl}}/images/node_red_fs_install.JPG)

4. Install moment node red module (if not present)
Based on a solution from this [post](https://discourse.nodered.org/t/msg-filename-logging-data/1117/13)

5. Install Homeassistant Samba module (if not already) and configure to access from win 10
Following [guide](https://riccardotramma.com/2018/10/use-samba-to-configure-home-assistant/) came in pretty handy.

## Configuring your timelapse

We are going to make all the shots at desired timeframe, for this we will use node red, if you want to import the flow, you can find it in the following Gist.
I used a 6 min interval, so i can have each 24h in a 20s video at 24fps.

<script src="https://gist.github.com/nkmakes/3e3807194b6f6c9a63831f0f5ad46a51.js"></script>

### General flow schematics

![Node-red timelapse general schematics]({{site.baseurl}}/images/timelapse_node.JPG)

You will need only 5 nodes. 2 timestamps, first one will be triggered everyday at 00:00 and will create a folder to save the photos of the day, and will erase the older folders, just saving the days we Need/Want.

The second timestamp will be triggered every x minutes, and will make a photo and will save it in the day folder.

### Retrieve the photos

At this moment i couldnt find any easy way to make the videos over my hassOS install.
So to create the timelapse we will need to download the images using a samba server. 
We can access the photos of each day at your configured location, in my case /config/timelapses

### Creating the timelapse

For this by the moment i'm using photoshop, as i'm retrieving the photos under a windows 10 OS and it's already installed, but there are plenty of other solutions available.

In my case i used the following video.

[Create time-lapse video from still images guide by Adobe](https://helpx.adobe.com/es/premiere-pro/how-to/create-time-lapse-sequence.html)


