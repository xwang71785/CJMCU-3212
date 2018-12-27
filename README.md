Guide to flash the CJMCU-3212. In Windows with Arduino IDE and NodeMCU flasher


# CJMCU-3212

![CJMCU-3212](https://i.imgur.com/z7lX4eF.jpg)

This board was originaly intended to inject payload scripts, like a Rubber Ducky (BadUSB), but if you want to use it for any generic project like me this guide will help you. 

## Board Features

The CJMCU-3212 has:
- ESP8266 
- Atmega32U4
- Micro SD slot with level shifter connected to Atmega32U4
- Button to put Atmega32U4 in bootloader flash mode (not a reset button)

## Pinouts

(To Do)

## Description 

The CJMCU-3212 features a ESP8266 connected through UART to the Atmega32U4, and the Atmega32U4 is connected to USB, to flash the ESP8266 we use the Atmega32U4 as a relay, making it retransmit everything to the ESP8266. 


## Resources used

You need the ESP boards added to your Arduino IDE
http://arduino.esp8266.com/stable/package_esp8266com_index.json

Also you need the nodemcu flasher
https://github.com/nodemcu/nodemcu-flasher


## 1: Put the Atmega32U4 as ESP8266 programmer

The first thing to do is put the Atmega32U4 as a serial relay and transmit to ESP, so in Arduino select: 

  *Tools-> Board-> Arduino Leonardo*

  *Tools-> Port -> (the corresponding port)*

And upload step1.ino from this repo

## 2: Compile your sketch for ESP8266

In Arduino IDE, compile the sketch you want to upload to ESP8266 with the following options selected

  *Tools-> Board-> Generic ESP8266*

  *Tools-> Flash Mode -> DIO*
  
  *Tools-> Flash Size -> 4M *  
  
  *Tools-> Flash Freq -> 80 MHz*
  
  *Tools-> Upload Speed -> 115200*

Then you can compile and export the binaries that wil be used to flash the ESP8266
 
  *Program -> Export binaries*


## 3: Upload the sketch on the ESP8266

Now, the Magic (or the solution that is working for now):

Disconnect the CJMCU.

We need to put the ESP8266 in flash mode (set GPIO0 LOW during boot of the board), to do so bridge the two metal circles on the top right before connect the CJMCU to the USB, you can use a jumper wire for example. Once connected to USB you can remove the bridge as the ESP8266 will stay in flash mode (image)

![](https://i.imgur.com/5ght4Uu.jpg)

Once connected, open Nodemcu flasher and select:

  *Advanced-> Baud -> 115200*

  *Advanced-> Flash size -> 4M*

  *Advanced-> Flash Speed -> 80MHz*
  
  *Advanced-> SPI Mode -> DIO*

Then in Config Tab select your compiled binary from step 2

Finally in Operation Tab select the right COM port and hit Flash button. Your ESP8266 sketch should be now flashed. 


## 4: (Optional) You can upload to Atmega32U4 any sketch you want.

For example, if you need Serial debugging of the ESP do step 1 again (For some reason step 3 "erases" the programming in the Atmega32U4). 

Also if you want to use the microSD slot in the board you'll need to upload the right sketch to the Atmega32U4 (the SD is wired to it, use chip select pin 8) 


## 5: (Suggestion) Upload an OTA-enabled Sketch to ESP

For those who don't know, OTA (Over The Air) library allows the ESP to be flashed wireless, so if you don't want to be following  the above steps every time you need to flash the ESP (you just flash it once by wired connection) you can follow the next guide: 
https://github.com/esp8266/Arduino/blob/master/doc/ota_updates/readme.rst . Then you just need the Arduino IDE, select the network port and upload directly. 

