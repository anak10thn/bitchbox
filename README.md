*DISCLAIMER:*
Please remember that I won't be held responsible for any damages or injury related to your building my projects.
Continue at your own risk knowing that I don't take responsibility for your actions.
Be safe and have fun!

*WARNING:*
This project is very difficult, even if you have knowledge of programming and linux. 
I completed this project a few years ago, and it was very difficult to get working.
All those I've helped build this project also had a hard time making it work.
Continue at your own risk, having been warned.


***IMPORTANT NOTE***

ALL my python scripts that I will ever write (probably) will be written in python 2.7, because that's what I know best.
PLEASE make sure that you run them with the correct python version (2.7).
This may change in the future, so I'll let you know if it does.

***IMPORTANT NOTE***

The included schematic files are meant to be opened with GEDA schematic editor. You can get it here:
www.gpleda.org

Rpifxbox.schem is the main schematic. This includes the wiring diagram for the potentiometers, push buttons, AD converter, and display.

Soundcard.schem is the schematic that shows how to wire up a USB soundcard to two 1/4" jacks (the kind on electric guitars and amps).

In this project I use a K107 R4 serial lcd backpack from Wulfden. It comes in kit form, and I highly reccomend it; you can buy one here:
http://www.wulfden.org/TheShoppe/k107/about.shtml

If you want to edit my program to use a different lcd backpack or just a plain old LCD, you can definatly do that. Just know that, in my program, I send '?f' out the serial line to clear the display, and 
'?n' to skip to a new line of the display. You will need to change these commands to fit whatever display configuration you choose to use.

The lcd I use is an Itron blue VFD.

In case you are unable to open the schematic files that I have included, please go here:
http://scruss.com/blog/2013/02/02/simple-adc-with-the-raspberry-pi/
To find the wiring diagram for connecting the MCP3008 ADC chip to the Pi.

Please note that I am powering the chip with 5v, unlike the website above where they use 3.3v. I also connected the VREF pin to 5v as well. This allows me to read up to a 5v signal.
The three push buttons that allow you to cycle through the effects are wired just as you would wire any other push button to the pi, with a 10k pulldown resistor going to GND, and a .2 ohm current limiting resistor going to the pi's gpio from the switch.

I connected the middle button to Raspberry pi pin 7(GPIO 4), the upper button to Raspberry pi pin 11(GPIO 17) and the lower button to Raspberry pi pin 12(GPIO 18).
Please note that the Raspberry pi's GPIO pins are not 5v tollerant. The maximum voltage you can put into them is 3.3v.
The soundcard I used can be bought here:
http://www.amazon.com/Syba-SD-CM-UAUD-Adapter-C-Media-Chipset/dp/B001MSS6CS
It has exceptional sound quality for an $8 card, and didn't need any special drivers, if I remember correctly.
I wired the input and output of this card to 1/4" jacks to make connecting an amp and guitar easy.


Please go here:
http://scruss.com/blog/2013/01/19/the-quite-rubbish-clock/#spi
to find a guide that walks you through setting up the spi bus on your raspberry pi. The ADC that I used in this
project uses the SPI protocol to communicate with the pi. This is why you need it set up.

Once you follow that guide, you need to install pure data extended:
www.puredata.info

***IMPORTANT NOTE***

We now have to change some file paths:
After installing puredata, double click the 'server.pd' file in the bitchbox folder. This should open puredata. 
You should see a box (it's actually called an object) that says:
```bash
pd open $1.pd /home/pi/Desktop/bitchbox
```
Press CTRL+E on your keyboard to go into edit mode. Now, click this object and change /home/pi/Desktop to be the path to wherever your bitchbox folder is. 
For example, if you cloned it to your home folder, you should change the path to /home/(your username here)/bitchbox
Just make sure it is the COMPLETE path to the folder. You will also need to change these lines:

```python
name_file = open ("/pi/Desktop/bitchbox/names.txt")

function_file = open ("/pi/Desktop/bitchbox/functions.txt")
```

In main.py to the FULL path to both names.txt and functions.txt respectivly.

***IMPORTANT NOTE***

While we have puredata open, let's configure the sound card: 
1. Click the 'Media' tab at the top of the puredata window.
2. Click 'Alsa'
3. Set 'Input device 1' and 'Output device 1' to what ever soundcard you are using. If you bought the same soundcard as I did, you should choose 'C-Media chipset', or something along those lines.
4. Click 'Apply' and 'OK'

One thing about some USB sound cards is that they can draw more current than older Raspberry pis can provide through their USB ports. I think this problem was fixed in the Rev. 2 board 
(the only one offically sold now), but if you have an older board, you might need to short out a polyfuse. It's what I did to my board, but I can't be responsible if you break yours:
http://theiopage.blogspot.com/2012/06/increasing-raspberry-pis-usb-host.html 
If you have weird popping or 8-bit sounding audio coming from your card, then under-current is probably the problem.


The only other peices of software you will need to install are pyserial and Rpi.GPIO (both are python libraries). If you are running a new version of Raspbian (Wheezy?) on your Raspberry pi, I think that these libraries come pre-installed. 
To make sure that all of the python libraries that you need are installed, type these commands at a python (version 2.7) shell window:

```python
import serial
import spidev
from time import *
import RPi.GPIO as GPIO
import os
import subprocess
```

If these commands don't throw an error, everything is installed. If one or two do throw an error, just google how to install them.
Now, assuming that all the needed libraries are installed, Just run my script 'main.py', and puredata should open. Feel free to play with the code and change the built in effcts! 

To add your own effect:

You will notice that inside the 'bitchbox' folder, there are two files called 'functions.txt' and 'names.txt'. Append a line to names.txt (the name of your effect), and append the controlls you wish to give your effect in functions.txt. Then open pure data, create your effect and save it as [number].pd. Each of the existing .pd files in the Rpieffect folder is named with a number (0 indexed), 1 through 15. Replace [number] in your effect's name with whatever number comes next. Example: if the files 0.pd 1.pd and 2.pd exist in the bitchbox folder, name your file 4.pd
Another example is this: If you leave all the effects that I made alone, you would name your file 16.pd

To controll your effect with the four pots, add this object to your effect:
```
netreceive  5001 1
```
connect it's first output to this object:
```
route 0 1 2 3
```
this route object's first four outputs will output the values of the four pots.
Connect them to a slider or number box object's input, and you can controll any four software effect parameters with physical hardware potentiometers.
you can multiply these values by a number to get a larger (or smaller) range, in exchange for some accuracy by connecting an output of 'route 0 1 2 3' to this object:
```
* [a number to multiply by]
```

example:
```
* 2 
```
or
```
* .75
```
or
```
* .002
```

If you don't understand any of that, you can always look at one of the .pd files that I wrote (for example, 2.pd) and copy/paste into your .pd file.

Just as a note, the potentiometers I used in my effect box are 10k ohms. I have found that audio taper pots don't work very well for fine adjustments. 
I would reccomend that you use linear pots. 10k seems like a good value, though. Experiment and see what you like!

And finally, because so many people ask for one, here is a BOM (Parts list):

1. Raspberry pi model B

2. Sound card as mentioned in this docment (Follow the amazon link)

3. Resistors (As per schematic diagram)

4. ADC chip ( look up "MCP3008" on Digikey.com, Mouser.com or even just google search it )

5. Three momentary push buttons

6. Four 10k ohm potentiometers

7. Lots of wire

8. 2 1/4" audio jacks (The type you'd find on a guitar amp)

9. Some perf board

10. A connector that will go from the raspberry pi's GPIO pins to the circuit (perf) board

11. An LCD serial backpack as mentioned before (Follow the Wulfden.com link)

12. An LCD that is compatible with the serial backpack

13. A 1x3 male .100" header to connect the serial backpack to the circuit (perf) board (RX, TX, and GND)

14. A way to wire the output of the sound card to the 1/4" jacks. I'll leave this up to you. I used some old clipped audio cables.

Well, I can't think of anything else, so I'll leave it at that.


Thanks for downloading my software,
Happy Hacking!

***P.S:*** If PD throws a soundcard related error, try running it with sudo. Also, make sure you have the extended version of PD.



 



