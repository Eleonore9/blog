---
layout: post
title: "Installing an Arduino Wifi shield"
date: 2014-11-12 17:11
comments: true
author: "Eleonore Mayola"
categories: [Arduino, Beginner]
---

After a few Arduino projects (mainly blinking LEDs and connecting sensors) I tried 
to connect my Arduino to the internet using a Wifi shield!

I started by looking at Arduino Wifi shield [instructions](http://arduino.cc/en/Guide/ArduinoWiFiShield).

Then I physically connected my Arduino and Wifi shield:

<img src="{{ root_url }}/images/arduino-wifishield.jpg" />

As a test I wanted to scan for available networks using the example code on [this](http://arduino.cc/en/Guide/ArduinoWiFiShield#toc5) 
Arduino Wifi shield webpage.

I mentioned in a previous blog [post](http://blog.opensensors.io/blog/2014/09/13/getting-started-with-arduino-on-linux/) 
I had an issue with the Arduino IDE and now use [inotool](http://inotool.org/). 
It enables to code for Arduino in your text editor/IDE of choice and compile and 
upload code from the command line (but it still needs the Arduino IDE to be installed).

Here's how I start a project with inotool:
<pre><code>~$ mkdir new_project && cd new_project</code>
<code>~$ ino init</code></pre>
Here's the structure of my project afterwards:

<img src="{{ root_url }}/images/directory_structure.png" />

I can then edit the code in src/sketch.ino. In that case I just copied/pasted 
the code from Arduino's example:

<img src="{{ root_url }}/images/Test_wifishield.png" />

I built as usual using this command: <pre><code>~$ ino build</code></pre> ... and got 
the following error:

<img src="{{ root_url }}/images/build_error_wifi.png" />

=> It seemed there was a conflict between the Ethernet and WiFi Libraries.

... Then I kind of lost it (O_O) and tried all those things:

* bypassing both Arduino IDE and inotool by using a Makefile for compiling Arduino (http://hardwarefun.com/tutorials/compiling-arduino-sketches-using-makefile)
     => and I got lost down makefile rabbit hole...
* updating (or trying to update) the wifi shield firmware using Arduino's [instructions](http://arduino.cc/en/Hacking/WiFiShieldFirmwareUpgrading) and blog posts.
    => when I finally managed it, it didn't have any effect...
* downgrading from Arduino IDE version 1.0.5 to 1.0.2 (older versions are found [here](http://arduino.cc/en/Main/OldSoftwareReleases)).
    => no improvement really...

I went back and forth between those solutions and at best I could compile the sketch but my MAC address was 0.0.0.0 (same for my IP address)!

**Here's how it finally worked for me:**

* I downgraded to Arduino IDE version 1.0.1.
* Copied "arduino-1.0.1" in usr/share/ and renamed it to "arduino" so that the path to the IDE was /usr/share/arduino/.
* I added wifishield/ folder from https://github.com/arduino/Arduino/tree/master/hardware/arduino/firmwares/wifishield to /usr/share/arduino/hardware/arduino/firmwares/.
* I added the WiFi library from https://github.com/arduino/Arduino/tree/master/libraries/WiFi to /usr/share/arduino.
* I upgraded the firmware (not sure if necessary) => *Note* : you need a USB 2.0 Mini B cable to link the wifi shield directly to your computer!
     - here are the instructions to install dfu-programmer: https://github.com/dfu-programmer/dfu-programmer
     - and you can get the latest wifi shield scripts from https://github.com/arduino/Arduino/tree/master/hardware/arduino/firmwares/wifishield/scripts
     - replace your scripts in /usr/share/arduino/hardware/arduino/firmwares/wifishield/scripts/ with the ones from Arduino Github repo.
     - close the J3 jumper on the shield (\*\*\*), 
         then run:
         <pre><code>~$ sudo ./ArduinoWifiShield_upgrade.sh -a /usr/share/arduino -f all</code></pre>

Yay! (^o^) You can now try new projects like creating a [webserver](http://arduino.cc/en/Tutorial/WiFiWebServer) or sending sensor data via internet!


<br>
(\*\*\*) In case you wonder what a **J3 jumper** looks like, 
those pictures could be useful to you:

- Here the J3 jumper is open (for when the shield communicate with the Arduino):

<img src="{{ root_url }}/images/j3_opened.jpg" />

- Here the J3 jumper is closed (or in programming mode, to update the firmware):

<img src="{{ root_url }}/images/j3_closed.jpg" />
