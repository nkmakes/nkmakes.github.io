---
layout: post
title:  "Esp32 tank robot joystick wifi controller"
date:   2020-9-2 8:05:55 +0100
image:  /images/entrada_tank.jpg
tags:   tank esp32 robot http javascript smars tb6612fng
description: Tank robot simple implementation with esp32, tb6612 and wifi. Async http web server control, using just one joystick to control the movement.
---

In this post, I will cover how to make a ESP32 tank robot Arduino sketch, using WiFi control. The idea is to control a small tank using http async web tecnologies, creating a webserver over the esp32 device that serves a JS based joytick. This JS app gathers the joystick position, remixes the results and sends the values needed for the motors of the tank.

I'll use this code for my [SMARS tank](https://nkmakes.github.io/2020/08/10/smars-esp32-wifi-robot-tank/) and for my [MR-6 tank](https://www.thingiverse.com/thing:2753227), for the later, as is based around ESP32-cam module, i'll also add to the guide in the future how to make the video stream.

The following video shows how it works.

<iframe width="560" height="315" src="https://www.youtube.com/embed/Z3jsNJ_2ksw" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

---

## 1.The Esp32 code

For this tutorial i'm going to use the code available on my [SMARS-esp32 repo](https://github.com/nkmakes/SMARS-esp32).

### Editing the Config

We weill need to tdith the /src/config.h file, we need to put the pins we are using in our case and changing your password.

{% highlight c++ %}
const char* ssid = "ESP32_ROBv1"; //Enter SSID
const char* password = "yourpasswd"; //Enter Password

#define AIN1 13
#define BIN1 12
#define AIN2 14
#define BIN2 25
#define PWMA 26
#define PWMB 27
#define STBY 17

// these constants are used to allow you to make your motor configuration
// line up with function names like forward.  Value can be 1 or -1
const int offsetA = 1;
const int offsetB = 1;

Motor motor2 = Motor(AIN1, AIN2, PWMA, offsetA, STBY,5000 ,8,1 );
Motor motor1 = Motor(BIN1, BIN2, PWMB, offsetB, STBY,5000 ,8,2 );
 
using namespace websockets;
WebsocketsServer server;
AsyncWebServer webserver(80);

int LValue, RValue, commaIndex;
{% endhighlight %}

### Importing our libraries
We will use for our sketch, the following libraries, they all need to be downloaded. They will be downloaded automatically if you are using PlatformIO. Here are the following links if you use Arduino program:
[ESPAsyncWebServer](https://github.com/me-no-dev/ESPAsyncWebServer)
[TB6612_ESP32](https://github.com/pablopeza/TB6612FNG_ESP32)

{% highlight c++ %}
#include <Arduino.h>
#include "Arduino.h"
#include <ArduinoWebsockets.h>
#include <WiFi.h>
#include <ESPAsyncWebServer.h>
#include <TB6612_ESP32.h>
#include "config.h"
#include "web.h"
{% endhighlight %}

### Creating the async webserver

in our /src/main.cpp we can find the following setup() code. It will create a Wifi hotspot and start a asyncwebserver using the [ESPAsyncWebServer](https://github.com/me-no-dev/ESPAsyncWebServer) library. This server will be usually in the 192.168.4.1 direction.

We could also modify this to connect to a home Wifi and control the tank, in this case it will have a different Wifi Adress based on your router settings.

This code will also make our esp32, on the port 80 deliver our webpage, which is compressed as the array index_html_gz[] in the /src/web.h file.

{% highlight c++ %}

void setup()
{
  Serial.begin(9600);

  // Create AP
  WiFi.softAP(ssid, password);
  IPAddress IP = WiFi.softAPIP();
  Serial.print("AP IP address: ");
  Serial.println(IP);

  // HTTP handler assignment
  webserver.on("/", HTTP_GET, [](AsyncWebServerRequest * request) {
    AsyncWebServerResponse *response = request->beginResponse_P(200, "text/html", index_html_gz, sizeof(index_html_gz));
    response->addHeader("Content-Encoding", "gzip");
    request->send(response);
  });

  // start server
  webserver.begin();
  server.listen(82);
  Serial.print("Is server live? ");
  Serial.println(server.available());
 
}
{% endhighlight %}

### Handling the clients and messages

To finish the sketch, we will need to control the motors based on the messages and connect to the clients if available.


{% highlight c++ %}
void handle_message(WebsocketsMessage msg) {
  commaIndex = msg.data().indexOf(',');
  LValue = msg.data().substring(0, commaIndex).toInt();
  RValue = msg.data().substring(commaIndex + 1).toInt();
  motor1.drive(LValue);
  motor2.drive(RValue);
}
 
void loop()
{
  auto client = server.accept();
  client.onMessage(handle_message);
  while (client.available()) {
    client.poll();
  }
}
{% endhighlight %}

## 2.The web Control

You can find a full webpage in the project under the /web_smars.html file, in the following steps i'll show you how i made it so you can made a new one for your needs!

### Finding a suitable jostick

To make the web interface control, we will need to use a joystick, this requires more advanced JS knowledge than i have at the moment, so i "borrowed" this wonderfull [jostick program from bobbotek](https://github.com/bobboteck/JoyStick)

You can also use any other or create your own! but this one is the one used with the sketch, and is pretty configurable, easy to setup and works pretty good.

This joystick is as easy to create as importing the module and adding the next to any html file:

{% highlight html %}

<div id="joyDiv" style="width:200px;height:200px;margin:auto;"></div>

{% endhighlight %}

### Getting the joystick values and calculating motor drive

Our joystick gives us -127..128 values, but our library uses -255..255 values, so we will need to convert the data to that values, that should be as easy as some maths, but we find also another problem.

We want to have a good control of the tank, and we can not just map one motor to the Y Axis and Another one to the X axis, as it would cause our tank to move straight in the right top corner (X=127;Y127) so we need to apply some further maths to it.

I will not duplicate this info as you can find it very well explained in the following [article](https://www.impulseadventure.com/elec/robot-differential-steering.html), the code i am using is adapted from authors C solution to JS, huge thanks for sharing.

This will lead as to the creation of a aditional function, called getFuerza, which will calculate, based on the input the motor drive power for each track.

This could be also calculated on the esp32 side, but using JS for this lets us free our ESP32 cores executing the code on the client.


You can find this code in the project /web_smars.html file.

{% highlight html %}
<script type="text/javascript">
    // Create JoyStick object into the DIV 'joyDiv'
    var joy = new JoyStick('joyDiv');
    var inputPosX = document.getElementById("posizioneX");
    var inputPosY = document.getElementById("posizioneY");
    var direzione = document.getElementById("direzione");
    var fuerzaI = document.getElementById("fuerzaI");
    var fuerzaD = document.getElementById("fuerzaD");
    var x = document.getElementById("X");
    var y = document.getElementById("Y");




    function getfuerza(nJoyX, nJoyY) {
        var nMotMixL;
        var nMotMixR;
        var fPivYLimit = 32.0; //The threshold at which the pivot action starts
        //                This threshold is measured in units on the Y-axis
        //                away from the X-axis (Y=0). A greater value will assign
        //                more of the joystick's range to pivot actions.
        //                Allowable range: (0..+127)

        // TEMP VARIABLES
        var nMotPremixL;    // Motor (left)  premixed output        (-128..+127)
        var nMotPremixR;    // Motor (right) premixed output        (-128..+127)
        var nPivSpeed;      // Pivot Speed                          (-128..+127)
        var fPivScale;       // Balance scale b/w drive and pivot    (   0..1   )

        // Calculate Drive Turn output due to Joystick X input
        if (nJoyY >= 0) {
            // Forward
            nMotPremixL = (nJoyX >= 0 ? 100.0 : 100.0 + parseFloat(nJoyX));
            nMotPremixR = (nJoyX >= 0 ? 100.0 - nJoyX : 100.0);
        } else {
            // Reverse
            nMotPremixL = (nJoyX >= 0 ? 100.0 - nJoyX : 100.0);
            nMotPremixR = (nJoyX >= 0 ? 100.0 : 100.0 + parseFloat(nJoyX));
        }

        // Scale Drive output due to Joystick Y input (throttle)
        nMotPremixL = nMotPremixL * nJoyY / 100.0;
        nMotPremixR = nMotPremixR * nJoyY / 100.0;

        // Now calculate pivot amount
        // - Strength of pivot (nPivSpeed) based on Joystick X input
        // - Blending of pivot vs drive (fPivScale) based on Joystick Y input
        nPivSpeed = nJoyX;
        fPivScale = (Math.abs(nJoyY) > fPivYLimit) ? 0.0 : (1.0 - Math.abs(nJoyY) / fPivYLimit);

        // Calculate final mix of Drive and Pivot
        nMotMixL = (1.0 - fPivScale) * nMotPremixL + fPivScale * (nPivSpeed);
        nMotMixR = (1.0 - fPivScale) * nMotPremixR + fPivScale * (-nPivSpeed);


        return Math.round(nMotMixL * 2.55) + "," + Math.round(nMotMixR * 2.55);   // The function returns the product of p1 and p2
    }

    // we set to 30 ms the send time
    setInterval(function () { send(getfuerza(joy.GetX(), joy.GetY())); }, 300);

</script>
{% endhighlight %}

---

Also we need the following code on the same file to send continuosly the messages to the web server.

{% highlight html %}
    <script>


        const view = document.getElementById('stream');
        const WS_URL = "ws://" + window.location.host + ":82";
        const ws = new WebSocket(WS_URL);

        ws.onmessage = message => {
            if (message.data instanceof Blob) {
                var urlObject = URL.createObjectURL(message.data);
                view.src = urlObject;
            }
        };


        var lastText, lastSend, sendTimeout;
        // limit sending to one message every 30 ms
        // https://github.com/neonious/lowjs_esp32_examples/blob/master/neonious_one/cellphone_controlled_rc_car/www/index.html
        function send(txt) {
            var now = new Date().getTime();
            if (lastSend === undefined || now - lastSend >= 30) {
                try {
                    ws.send(txt);
                    lastSend = new Date().getTime();
                    return;
                } catch (e) {
                    console.log(e);
                }
            }
            lastText = txt;
            if (!sendTimeout) {
                var ms = lastSend !== undefined ? 30 - (now - lastSend) : 30;
                if (ms < 0)
                    ms = 0;
                sendTimeout = setTimeout(() => {
                    sendTimeout = null;
                    send(lastText);
                }, ms);
            }
        }

    </script>

{% endhighlight %}



### Getting our web changes to the ESP32

To display our modified webpage, we need to add the compressed code to the /src/web.h file. For this it needs to be first compressed, you need to add all of your webpage code to the input field in the following [web baker](https://gchq.github.io/CyberChef/#recipe=Gzip('Dynamic%20Huffman%20Coding','index.html.gz','',false)To_Hex('0x',0)Split('0x',',0x')).

Then you will need to copy all of the output data, withouth the first "," inside of the src/web.h file, as content of the index_html_gz[] array.


## Conclusion

If you followed this tutorial, you should be able to control a esp32 robot based tank using a webserver and a jostick, if you find any problems following the guide or you think anything needs any change feel free to add any comments.

You can see some short videos of the SMARS tank using this code. As you can see its pretty easy to control and responsive.


<iframe width="560" height="315" src="https://www.youtube.com/embed/uIImwilvI2s" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>












