# Example of ESP32 D1 Mini + RS232 interface

This is an example of the assembly situation for the ESP32 D1 Mini and the RS232 inteface board mounted on a carrier board.

<img src="https://github.com/GernotAlthammer/buderus_ecomatic4000/blob/main/Example/IMG_7357.jpg" style="width: 40%;"><img src="https://github.com/GernotAlthammer/buderus_ecomatic4000/blob/main/Example/IMG_7358.jpg" style="width: 40%;">
<img src="https://github.com/GernotAlthammer/buderus_ecomatic4000/blob/main/Example/IMG_7359.jpg" style="width: 80%;">


The ESP32 and the RS232 board are mounted side by side on a carrier board that is soldered with a few pins onto the M404 small daugther board.

<img src="https://github.com/GernotAlthammer/buderus_ecomatic4000/blob/main/Example/IMG_7350.jpg" style="width: 80%;">


The carrier board (50 x 70 mm) does have a few pins to mount and connect the ESP32 and the RS232, as well as the signal routing.<br/>
**Note:** the RX/TX from ESP32 to the RS232 module is **cross-over!**

ESP32 OI ------ connection ----- TTL-RS232 module (TTL pins)<br/>
RXD - GPIO3 --------------------- Pin TX -><br/>
TXD - GPIO1 --------------------- Pin RX <-<br/>
GND - GND ---------------------- Pin GND<br/>
VCC - VCC ------------------------ Pin VCC<br/>

<img src="https://github.com/GernotAlthammer/buderus_ecomatic4000/blob/main/Example/IMG_7353.jpg" style="width: 80%;">


The power supply for ESP32 and RS232 can be taken from the M404 daughter board - comming from the M404 main board. 

<img src="https://github.com/GernotAlthammer/buderus_ecomatic4000/blob/main/Example/IMG_7354.jpg" style="width: 80%;">


This is the used RS232 interface module and pin-out

<img src="https://github.com/GernotAlthammer/buderus_ecomatic4000/blob/main/Example/IMG_7356.jpg" style="width: 60%;">

The M404 module with the additional ESP32 + RS232 does fit nicely into the slot.<br/>
**Note:** Because of the RX/TX cross-over between the ESP32 and the TTL-RS232 module the signal lines between the two D-Sub connectors are **1-to-1** and not cross-over anymore!.

TTL-RS232 module --- color --------- RS232 of KM2.0 <br/>
Pin 2 - RX ------------ orange -------- Pin 2 - RX<br/>
Pin 3 - TX -------------- red ---------- Pin 3 - TX<br/>
Pin 5 - GND ---------- brown -------- Pin 5 - GND<br/>

<img src="https://github.com/GernotAlthammer/buderus_ecomatic4000/blob/main/Pictures/IMG_3732.JPG" style="width: 80%;">


**Note:** I have not tested if the RS485 interface is working.<br/>
It may require to change the setting of the DIP-switch S4 (default: switch 1 + 2 OFF) or S3 (default switch 1 + 2 ON).
<img src="https://github.com/GernotAlthammer/buderus_ecomatic4000/blob/main/Pictures/M404_Front_Devices.JPG" style="width: 80%;">

.
