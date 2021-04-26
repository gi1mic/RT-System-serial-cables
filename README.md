# # RT-System-serial-cables
RT-System serial cable replacments


## Background

I recently became the proud owner of a used ICOM ID-5100. The rig came with an original CD of the rt SYSTEMS WCS-D5100-Data cloning software, a USB-29A programming lead and a generic programming lead for use with ICOMs software. The software also came with a few random cables plus licenses for a few transceivers including a YAESU FT817 (no cable, but more on that later).<br><br>

The problem was no matter what I could not get the rt SYSTEMS software to communicate with the ID-5100 using the USB-29A programming lead and much to my annoyance rt SYSTEMS in their great wisdom do not work with  cables from anyone else.<br><br>

Keen to try the rt SYSTEMS software I opened the USB-29A cable to find the ubiquitous FTDI 232RL chip plus a few surface mount transistors to perform the RS232 level matching. A little bit of testing and it appeared one or more of the surface mount transistors must have failed.<br><br>

At this point I could have tried to repair the USB-29A but instead I decided it would be quicker and easier to reprogram the generic lead I already had which by chance was also based on the FTDI 232RL.<br><br>

## Making the USB-29A replacement

First download the FT_Prog application from the https://ftdichip.com/utilities/ website.<br>

This application is used by system designers to configure FTDI 232RL chips.
Using the FT_Prog program I scanned the USB-29A to get the critical USB string descriptors and the USB vendor ID (VID) and product ID (PID).<br>

- Manufacture = RT Systems
- Product Description = CT29A Radio Cable
- Serial Number = RTxxxxxx
- VID = 0x2100
- PID = 0x9E53<br>

Note: The same information is available from the Linux FTDI serial databases if you google it.<br>

The most critical part of the process is finding a FTDI USB to serial board with chip that can re-programmed. This is because a few years ago FTDI introduced code into the Windows FTDI driver to change the PID of fake FTDI devices to 0x0000 effectively making them useless. It seems to avoid this current fake FTDI devices simply ignore any requests to change their settings.<br>

How do you know if you got a fake? Good question. Try reading one of the many articles on the internet 😊<br>

I was lucky the generic CI-V programming cable provided had a “real” FTDI chip along with a MAX232 level shifter. Another physically identical cable I had, marked LD-C101, was not based on an FTDI chip.

<center><img src="https://github.com/gi1mic/RT-System-serial-cables/blob/main/images/ci-v%20cable.jpg?raw=true" width=50%></center>
<br><br>

## Re-programming the cable
Start the FT_Prog application without any FTDI board being plugged in. Then hit F5 to start a FTDI scan to make sure nothing is attached. If anything is found be careful - you do not want to reprogram the wrong device!
Plug in the FTDI board, wait a few seconds for windows to identify it and then press F5 again. You should now get a screen something like the following.<br>

<center><img src="https://github.com/gi1mic/RT-System-serial-cables/blob/main/images/ftdi.jpg?raw=true" width=50%></center>

Now press CTRL+S to save the newly read device as a template. This will help recover the device should anything go wrong 😊<br><br>

Now change the Vendor ID and the Product ID to

VID = 2100
PID = 9E53

<center><img src="https://github.com/gi1mic/RT-System-serial-cables/blob/main/images/vid-pid.jpg?raw=true" width=50%></center>
<br><br>

Then update the string descriptors to:
Manufacture = RT Systems
Product Description = CT29A Radio Cable
Serial Number = RTxxxxxx
<br><br>

<center><img src="https://github.com/gi1mic/RT-System-serial-cables/blob/main/images/string%20desc.jpg?raw=true" width=50%></center>
<br><br>

Warning make sure you do not change the “Use External Oscillator” setting. Setting external oscillator, without one fitted, will stop the board working. If this happens you will need to feed a 12Mhz signal into pin 27 to recover it. How do I know - from trying to copy settings from a board that used an external OSC to one that did not!

<center><img src="https://github.com/gi1mic/RT-System-serial-cables/blob/main/images/ext%20osc.jpg?raw=true" width=50%></center>
<br><br>

With the correct information set press CTRL-P to program your device. This should only take a few seconds.<br>

Once programmed remove, reinsert, and then scan again to make sure the changes have been made. Fake devices will revert the FTDI defaults!<br>


## Wiring
Since I was re-using an existing CI-V cable for the ID-5100 there was no wiring to be done. But you could use a FTDI USB to serial board along with a MAX232 level shifter to achieve the RS232 levels for the same result.


### 2.5mm Jack
- Tip 	data out from ID-5100
- Ring 	data to ID-5100
- Body 	GND

<center><img src="https://github.com/gi1mic/RT-System-serial-cables/blob/main/images/ICOM%20ID-5100%20data%20port.jpg?raw=true" width=50%></center>

## Usage
You will need to install the USB-29A drivers for the newly programmed cable to be identified by windows as a serial device. The cable then plugs into the data port on the ID-5100 and can be used by either the rt Systems software or the ICOM software.

<center><img src="https://github.com/gi1mic/RT-System-serial-cables/blob/main/images/ICOM%20ID-5100.jpg?raw=true" width=50%></center>
<br>

## YAESU FT817 CAT 62B Radio Cable

Having made I replacement USB-29A cable and since I had a YAESU FT-817 plus license key I thought I would try creating a 62B Radio Cable. This is simpler since the FT-817 CAT interface operates at 5V levels and just needs a FTDI breakout board (one where the FTDI chip than can be reprogrammed!).<br>

The process is the same as above but this time the details are:<br>

- Manufacture = RT Systems
- Product Description = CT-62B Radio Cable
- Serial Number = RTxxxxxx
- VID = 0x2100
- PID = 0x9E56
<br>

Note: Again, the same information is available from the Linux FTDI serial databases if you google it.
<br><br>

## Wiring
The following diagram from https://github.com/4x1md/ft817_cat_python shows the required wiring. The board I used is identical to the one shown in the image and is marked FTDI232 on the underside. An identical board marked YP-05 that I happened to have could not be reprogrammed.<br>

The board jumper should be configured for 5V as shown in the image.
<br>

<center><img src="https://github.com/gi1mic/RT-System-serial-cables/blob/main/images/FTDI%20to%20FT817.jpg?raw=true" width=80%></center>
