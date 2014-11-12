---
layout: post
title: "Getting started with Arduino on Linux (Ubuntu)"
date: 2014-09-13 07:56
comments: true
author: "Eleonore Mayola"
categories: [Arduino, Beginner]
---

Ordered and received your Arduino Uno (I actually received it a while ago!) now what? The first step is to rush to the ['Getting Started'](http://arduino.cc/en/Guide/HomePage) page on Arduino's website. 

To install Arduino on Linux (I'm running Ubuntu 14.04) visit the [wiki page](http://playground.arduino.cc/Learning/Linux) with instructions for most Linux distributions. I used the instructions for Ubuntu 12.04 or newer and ran <pre><code>~$ sudo apt-get update && sudo apt-get install arduino arduino-core</code></pre> 
Afterwards you should be able to plug your Arduino and use Arduino IDE to code/upload code to it. It didn't work directly for me. I couldn't launch Arduino IDE by clicking the logo so I tried the CLI <pre><code>~$ arduino</code></pre> Here's the error it returned:

> Exception in thread "main" java.lang.ExceptionInInitializerError.

<img src="{{ root_url }}/images/arduino_bug1.png" />

I found a solution on [AskUbuntu](http://askubuntu.com/questions/26474/unable-to-install-arduino) forum. I downgraded from java 7 to java 6 __(\*\*\*)__ using the command: <pre><code>~$ sudo update-alternatives --config java</code></pre>
I then managed to launch Arduino from the CLI (I was asked to authenticate) and tried to upload a code example to my Arduino (Files > Examples > 01.Basics > Blink).
I received this error message: 

> processing.app.SerialNotFoundException: Serial port 'COM1' not found. Did you select the right one from the Tools > Serial Port menu?

<img src="{{ root_url }}/images/arduinobug2.png" />

In Tools I saw that Serial Port was greyed out so I couldn't even select the port.
from [Stackoverflow](http://stackoverflow.com/questions/19322432/arduino-tools-serial-port-greyed-out) I found that to give read and write privilege on the usb port I could run Arduino with the administrator privileges.
And so I used <pre><code>~$ sudo arduino</code></pre> -> It worked! It blinked!
Here's a proof:

<img src="{{ root_url }}/images/arduino_blinks.jpg" />

Troubleshooting
--------------
* Check your Arduino IDE settings: Tools > Board (the right borad is select), Tool > Serial Port (the right port for your device is ticked).
* Check all dependencies were downloaded with arduino and arduino-core .
  * Arduino's dependencies: arduino-core, default-jre, libjna-java, librtx-java
  * Arduino-core's dependencies: avr-libc, avrdude, gcc, gcc-avr
* Also try LadyAda's [help page](http://www.ladyada.net/learn/arduino/help.html). 



__(\*\*\*) Well, well. You might realise later that you actually need java 7 for another program to work! But fear not, see the alternatives below.__

Alternatives to Arduino IDE
--------------------
[Arduino IDE](http://arduino.cc/en/main/software) is based on java but luckily there other tools to write code for Arduino.

I tried [Ino](http://inotool.org/) which is written in Python and is a command line tool and allows to write code in any text editor.
It is straightforward to install with instructions given. You will need to install picocom program if you don't have it <pre><code>~$ sudo apt-get install picocom</code></pre>
There are other [alternatives](https://learn.sparkfun.com/tutorials/alternative-arduino-interfaces) to the Arduino IDE like [Codebender](https://codebender.cc/) which is an online development platform.
