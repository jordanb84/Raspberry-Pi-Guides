---
layout: post
title:  "PiHole + PiVPN + DNSCrypt"
---

# Setting Up 

##### To be clear, you do not need all 3, I will explain what you do/dont need to do for each of them if you dont want to do all of them.
 Oh and NOOBS and raspian are the same thing. NOOBS makes it far less of a pain. 
 **Skip to PiVPN/OpenVPN if youve installed the OS and gotten it set up**
 
1. Grab a Micro SD card if youre using a Pi2/Pi3.(You can skip this if youve already freshly installed Raspian/NOOBS). If its over 32GB, use a formating utility like GUIFormat, if the SD card is less than 32GB, windows can format it.
Select FAT32, and make sure nothing is on the card before you format it. 

2. [Download the OS](https://www.raspberrypi.org/downloads/) 
*I reccomend using the Lite varient for this project but the Desktop version will work if you want a GUI.*
If you choose NOOBS (ez Raspian Installer), you cant use PiBakery
If you choose the .ISO/.IMG of Raspian, you can download PiBakery and use it [Here](https://www.raspberrypi.org/downloads/).
Installing alone can take 10-15 minutes. So make sure youve got a few hours set aside. 

3. I reccommend enabling at the very least SSH, and changing your locales, timezone, and keyboard (the GB one is different than the US one), if you have a PI3, you can bump up your arm-freq (```sudo nano /boot/config.txt```) which is essentially the clock speed of the pi. (Changeing the clock isnt neccary at all, if you dont want/need to, just skip this.) The default is 700 or 800, but you can set that up to 1000 or even to the 1400 it should be at. (Provided you have a good source of power for the pi.) If the pi doesnt boot, dont freq out, (haha) just hold shift on boot to get to the recovery (you need the pi hooked up to a monitor/mouse & keyboard for this) and you can edit the confix.txt directly fromt he menu. Or power off the pi, pull the sd card, and plug it into another computer, do to the /BOOT partition, and edit the config.txt and turn it down to 1200. **Also** if you went the ISO/IMG route, make sure you expand for filesystem in ```raspi-config``` so you can use all the space on your SD card. (NOOBS does this already.)

4. Once thats all set up and youve rebooted, type ```ip a``` (formally known as ifconfig). the eth0 interface is for if the pi is connected via ethernet. Wlan0 is for WI-FI. Look under the interface for how your pi is connected, and if its a 192.168.x.x number, yay, if not, well you've got some troubleshooting to do. 

5. Now, set a static IP, edit your wpa_supplicant.conf with ```/etc/wpa_supplicant/wpa_supplicant.conf```. [This](https://www.raspberrypi.org/documentation/configuration/wireless/wireless-cli.md) should cover everything for the wifi side. 
![alt text][eth0-setup]

[eth0-setup]: https://cdn.discordapp.com/attachments/323186380379389952/591795416069046293/unknown.png 
Its easiest to use your pis current ip, then for static routers its usually 192.168.1.1 or 192.168.1.254 (Generally AT&T routers only), but you can check with windows, type ```ipconfig``` in cmd and its labled as Default Gateway. 
For the DNS, it is advisable to delete whats there and use either ```8.8.8.8``` or ```1.1.1.1``` (Googles or Cloudflares respectivly)
In the default configuration there is an option to set the IPv6, theres really no use for it so that line can be deleted. 
Under the "Example static IP configuration" set you can uncomment all of that and use that to save you a bit of time. 
Once thats changed, Ctrl+X, Y, then hit enter and the file will be saved.

Reboot and with ```ip a``` check to make sure the settings are good (and also write it down as youll need it for SSH as well as asigning a static IP for pihole. Then just to be sure ```ping google.com``` and if you get a responce, Ctrl+C and onto installing everything!

# PiVPN / OpenVPN
***WIP***

