---
layout: post
title:  "Installing and Setting Up Your RPi"
---


# Raspian / NOOBS installation and setup

### This guide is intended as, well, a guide, while there are offical ones we feel its easier to condense a bunch of things into one somewhat small guide as well as reference steps. 


1. Grab a Micro SD card if you're using a Pi2/Pi3.(You can skip this if you've already freshly installed Raspian/NOOBS). If its over 32GB, 
use a formatting utility like GUIFormat, if the SD card is less than 32GB, windows can format it.
Select FAT32, and make sure nothing is on the card before you format it. 

2. [Download the OS](https://www.raspberrypi.org/downloads/) 
If you choose NOOBS (ez Raspian Installer), you can't use PiBakery, however our guide for PiBakery is [Here](https://jordanb84.github.io/Raspberry-Pi-Guides/2019/06/10/Pibakery.html). If you choose the .ISO/.IMG of Raspian, you can download PiBakery and use it [Here](https://www.raspberrypi.org/downloads/).
Installing alone can take 10-15 minutes. So make sure you've got a bit on time to set this up. 

3. I recommend enabling at the very least SSH, and changing your locales, timezone, and keyboard (the GB one is different than the US one), 
if you have a PI3, you can bump up your arm-freq (```sudo nano /boot/config.txt```) which is essentially the clock speed of the pi. 
(Changing the clock isn't necessary at all, if you don't want/need to, just skip this.) The default is 700 or 800, 
but you can set that up to 1000 or even to the 1400 it should be at. (Provided you have a good source of power for the pi.) 
If the pi doesn't boot, don't freq out, (haha) just hold shift on boot to get to the recovery 
(you need the pi hooked up to a monitor/mouse & keyboard for this) and you can edit the confix.txt directly from he menu. 
Or power off the pi, pull the sd card, and plug it into another computer, do to the /BOOT partition, 
and edit the config.txt and turn it down to around 1200. **Also** if you went the ISO/IMG route, 
make sure you expand for filesystem in ```raspi-config``` so you can use all the space on your SD card. (NOOBS does this already.)

4. Once that's all set up and you've rebooted, type ```ip a``` (formally known as ifconfig). 
The eth0 interface is for if the pi is connected via Ethernet. wlan0 is for WI-FI. 
Look under the interface for how your pi is connected, and if its a 192.168.x.x number, yay, if not, 
well you've got some troubleshooting to do. 

5. Now, set a static IP, edit your wpa_supplicant.conf with ```/etc/wpa_supplicant/wpa_supplicant.conf```. [This](https://www.raspberrypi.org/documentation/configuration/wireless/wireless-cli.md) 
should cover everything for the wifi side. 
![alt text][eth0-setup]

[eth0-setup]: https://cdn.discordapp.com/attachments/323186380379389952/591795416069046293/unknown.png 
Its easiest to use your pis current ip, then for static routers its usually 192.168.1.1 or 192.168.1.254 (Generally AT&T routers only), 
but you can check with windows, type ```ipconfig``` in cmd and its labeled as Default Gateway. 
For the DNS, it is advisable to delete what's there and use either ```8.8.8.8``` or ```1.1.1.1``` (Googles or Cloudflares respectively)
In the default configuration there is an option to set the IPv6, there's really no use for it so that line can be deleted. 
Under the "Example static IP configuration" set you can uncomment all of that and use that to save you a bit of time. 
Once that's changed, Ctrl+X, Y, then hit enter and the file will be saved.

Reboot and with ```ip a``` check to make sure the settings are good (and also write it down as you'll need it for 
SSH as well as assigning a static IP for later projects and SSH. Then just to be sure ```curl ifconfig.me``` and if you get your
Public IP, then you're ready to move on!
