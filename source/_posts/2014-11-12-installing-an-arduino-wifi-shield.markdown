---
layout: post
title: "Installing an Arduino Wifi shield"
date: 2014-11-12 17:11
comments: true
author: "Eleonore Mayola"
categories: [Arduino, Beginner]
published: false
---

After a few Arduino projects (mainly blinking LEDs and connecting sensors) I tried 
to connect my Arduino to the internet using a Wifi shield.

I started by looking at Arduino Wifi shield instructions: http://arduino.cc/en/Guide/ArduinoWiFiShield.
Then I physically connected my Arduino and Wifi shield:

<picture arduino+wifi shield>

As a test I want to try the example code on this Arduino Wifi shield webpage to 
scan for available networks (http://arduino.cc/en/Guide/ArduinoWiFiShield#toc5).

I mentioned in a previous blog post (http://blog.opensensors.io/blog/2014/09/13/getting-started-with-arduino-on-linux/) 
I had an issue with the Arduino IDE and now use inotool (http://inotool.org/). 
It enables to code for Arduino in your text editor/IDE of choice and compile and 
upload code from the command line (but it still need the Arduino IDE to be installed).

Here's how I start a project with inotool:
<pre><code>~$ mkdir new_project && cd new_project</code>
<code>~$ ino init</code></pre>
Here's the structure of my project after that:

<picture ino project structure>

I can then edit the the code in src/sketch.ino. In that case I just copied/pasted 
the code from Arduino's example:

<picture example code>

I built as usual using the command <pre><code>~$ ino build</code></pre> and got 
the following error:

<picture error with Ethernet>

=> It seems there's a conflict between the Ethernet and WiFi Libraries.

... Then I kind of lost it and tried all those things:

* bypassing Arduino IDE and inotool and using a Makefile for compiling Arduino (http://hardwarefun.com/tutorials/compiling-arduino-sketches-using-makefile)
     => I got lost down makefile rabbit hole
* updating (or trying to update) the wifi shield firmware using Arduino's instructions (http://arduino.cc/en/Hacking/WiFiShieldFirmwareUpgrading) and blog posts.
* downgrading from Arduino IDE version 1.0.5 to 1.0.2 (older versions are found here: http://arduino.cc/en/Main/OldSoftwareReleases).

I tried those all at once and at best I could compile the sketch but my MAC (and IP) addresses was 0.0.0.0!

Here's how it finally worked for me:

* I downgraded to Arduino IDE version 1.0.1.
* Copied "arduino-1.0.1" in usr/share/ and renamed it to "arduino" so that the path was again /usr/share/arduino/.
* I added wifishield/ from https://github.com/arduino/Arduino/tree/master/hardware/arduino/firmwares/wifishield to /usr/share/arduino/hardware/arduino/firmwares/
* I added WiFi library from https://github.com/arduino/Arduino/tree/master/libraries/WiFi to /usr/share/arduino
* I upgraded the firmware (not sure if necessary) => Note: you need a USB 2.0 Mini B cable to link the wifi shield directly to your computer!
     - here are the instructions to install dfu-programmer: https://github.com/dfu-programmer/dfu-programmer
     - and you can get the latest wifi shield scripts from https://github.com/arduino/Arduino/tree/master/hardware/arduino/firmwares/wifishield/scripts
     - replace your scripts in /usr/share/arduino/hardware/arduino/firmwares/wifishield/scripts/ with the ones from Arduino Github repo.
     - close the J3 jumper on the shield  
         Then run:
         <pre><code>~$ sudo ./ArduinoWifiShield_upgrade.sh -a /usr/share/arduino -f all</code></pre>
