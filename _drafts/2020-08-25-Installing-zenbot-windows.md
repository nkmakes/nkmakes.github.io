---
layout: post
title:  "Zenbot part 1: Running our open source crypto bot on Windows 10"
date:   2020-8-25 8:05:55 +0100
image:  /images/10.jpg
tags:   Zenbot crypto bitcoin trading bot
description: How-to guide to create your own local Zenbot crypto bot to start to test crypto strategies today.
---


## Intro about open source crypto trading bots

### What is a cripto bot?

### Open source crypto bots as 2020

## Zenbot local setup

### Initial requeriments
Updated Windows 10 or Windows 10 Home OS with git

### Setting up our Docker enviroment on Win 10

We need first to set up a docker enviroment on Windows 10. If you have Home edition OS, you will need to [install WSL 2 or Windows susbsistem for Linux](https://docs.microsoft.com/en-us/windows/wsl/install-win10). Before this you need to have your OS updated to the last version to avoid problems.

If you are lucky, you can directly install [Docker install Full Guide](https://docs.docker.com/docker-for-windows/install-windows-home/)


### Installing our bot docker container

Once we got our Docker Desktop program installed, we need to setup our Docker container, for this we need to move to a folder using the terminal.


Docker Desktop on its first run tutorial, also offers us a hendy terminal to execute this commands.

In my case ill move to /Users/xxx/onedriveImages to have them automatically saved on the cloud.

Once we are there, we need to copy using git the docker files with


git clone https://github.com/deviavir/zenbot.git
cd zenbot

Once there we need to docker image mongo version line on docker-componse.yml

from:
mongodb-data:
  image: mongo:latest

to:
mongodb-data:
  image: mongo:4.0
  volumes:

And we needto copy the file conf-sample.js to a new file called conf.js. This will be our coinfuration file.
You can take a look at it now but i'll cover it more in depth later


Once we had all the wanted changes at this build we can componse the image with the following command:

docker-compose --file=docker-compose-windows.yml up












 [Smars modular robot project][smars-link].
 [file editor](https://github.com/home-assistant/hassio-addons/tree/master/configurator) 

{% highlight yaml %}
homeassistant:
  #other config lines
  #...
  #...
  #end of other config lines
  whitelist_external_dirs:
  - /config/timelapses
{% endhighlight %}


![Hass homeassistant timelapse photo check]({{site.baseurl}}/images/hass_service_test.png)


[smars-link]: https://www.thingiverse.com/thing:2662828


