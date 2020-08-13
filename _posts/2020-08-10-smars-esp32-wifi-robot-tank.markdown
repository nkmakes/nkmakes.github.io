---
layout: post
title:  "SMARS esp32 wifi robot tank"
date:   2020-8-10 8:05:55 +0100
image:  10.jpg
tags:   esp32 projects
---

## Intro
Wanted a small simple robot to test different control modules and after some search, i landed into [Smars modular robot project][smars-link].
This small robot is based around a 9V battery and a arduino uno board, i had laying around some esp32 based arduino uno sized board and a screw shield terminal so it looked like a perfect fit, i had already everything needed excepting some cheap 6V motors.

## 3d print and build
The model is very easy to print and build, no supports needed, had some problems when mounting the tracks but with a little bit of oil in the idle wheel everything works really well. In the parts of the original models uses 200rpm motors, mine are 300rpm as the other ones where not available, but its not any problem

## Electronics
As mentioned i used a esp32 arduino sized board and mounted over it a little homemade shield with a tb6612 driver and external VIN

## Code
{% highlight c++ %}

Motor motor2 = Motor(AIN1, AIN2, PWMA, offsetA, STBY,5000 ,8,1 );

Motor motor1 = Motor(BIN1, BIN2, PWMB, offsetB, STBY,5000 ,8,2 );

 

using namespace websockets;

WebsocketsServer server;

AsyncWebServer webserver(80);

 



 

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

 

// handle http messages

void handle_message(WebsocketsMessage msg) {

 int commaIndex = msg.data().indexOf(',');

 int LValue = msg.data().substring(0, commaIndex).toInt();

 int RValue = msg.data().substring(commaIndex + 1).toInt();

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

## Some action
<iframe width="560" height="315" src="https://www.youtube.com/embed/Z3jsNJ_2ksw" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
<iframe width="560" height="315" src="https://www.youtube.com/embed/uIImwilvI2s" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>


[smars-link]: https://www.thingiverse.com/thing:2662828


