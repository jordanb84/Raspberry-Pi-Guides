---
layout: post
title:  "PiHole + PiVPN"
---

# Setup

A guide was made to cover most of what you need for the basic setting up of the pi. However for this I recommend you use the Lite version as you'll have more system resources available (not that pihole or pivpn use that much). [Here's the guide](https://jordanb84.github.io/Raspberry-Pi-Guides/2019/06/22/installingsettingup.html) to that and once you've gotten what you need, you can being by installing PiHole.

This tutorial is easier when done with SSH and console autologin enabled. 

# Installing PiHole

The lovely lads over at the [PiHole website](https://pi-hole.net/) have an automated installer script that will guide you through it. 
(Important: Make sure you have a picture/screenshot of the Pis IP as well as what your default gateway IP is. You'll need these for later. This can be done with `IP a`.)
1) Run the command `sudo curl -sSL https://install.pi-hole.net | bash`. You want to use sudo as this requires root. Let that run and you should get a dialogue saying "This installer will transform your device into a network-wide ad blocker!"
Hit enter, it'll ask for donations, just hit enter again, it will explain you need to set up a static IP as it is a server. Hit enter again. 
2) Itll ask you what interface you would like to use. If you are using wifi to connect to the internet, with the wlan0 (tap the down key and press the spacebar, then enter), BUT if you're using ethernet (a cable), then just press enter. 
3) Now itll ask you what DNS you want to use, I recommend CloudFlare, but if you want to use Goggles DNS, just press enter, otherwise hit the down arrow until you get to the DNS provider you want. 
4) Itll ask what lists you want to use, just press enter, these lists arent like others that can mess up web pages loading. 
5) You choose where you want to block ads from, (IPv4 and IPv6). Just press enter. 
6) Now heres where you want to use the IP, it will list your current network settings and if they match up, just press enter. If you know what you are doing and decide you want to change the IP, select no and it will go through changing it. 
7) Hit enter as this slide explains some routers may still try to assign the pi an alternate IP, but most don't. 
8) This slide just states what your IPv6 is if the blocking for that is supported. 
9) This is where it asks fi you want to install the web admin interface, its easier and you get the teleporter for if you want to share blocklists. I suggest installing this, it makes like easier (except if you're using something else that requires a web interface). Also hit enter for the next slide as you need the web server for the web interface. 
10) Hit enter again, this just asks if you want logs. This can be disabled if you change your mind but I suggest leaving it enabled. 
11) The dialogue explains this but its asking what privacy level you want, [here](https://docs.pi-hole.net/ftldns/privacylevels/) is a list of specifics for those that have a specific use case but otherwise I would leave it at 0 as you'll get the most detail in the web interface. 

And finally it will install everything, it shouldn't take too long. 
12) The install will finish and this screen you want to take note of as this gives you the IP you will use for the DNS as well as where to log into the web admin interface, and the password for it. 

13) I highly recommend changing the admin password for the interface (and for the pi itself) you can use `pihole -a -p` to change the web admin password. 

# Installing PiVPN

As with PiHole, PiVPN is just as simple. A single command. `sudo curl -L https://install.pivpn.io | bash`. 
1) Run the command, let it do its thing, and it greets you, hit enter, then says itll require a static IP as PiHole does, hit enter, since this was set up with PiHole, just hit enter again. Then enter again to skip the IP changing warning. 
2) hit enter again to proceed to the using that can manage the OVPN files (its how you'll connect to the vpn sever)
3) Hit enter again as `pi` is the only user. 
4) Hit enter, this reminds you you'll need to set a port later. 
5) Hit enter to enable unattended upgrades
6) let it do some stuff then just hit enter. As explained by the dialogue, you don't need to TCP unless you know why you need it. 
7) here you'll set the port you'd like to use. It can be the default one BUT, it is highly recommended that you set it to something in the 49,152 to 65,535 range as those arent used by any applications so they wont interfere with anything else you might have going on. It doesn't matter which of these you use so long as you can remember what it is long enough to use it to port forward the server (which whill be covered later)
8) Hit enter to enter the port, then enter again to confirm
9) If you don't care about encryption and just need a VPN, hit enter, the default is a 2048 bit RSA key. If you feel paranoid, hit the down arrow and hit space to select the 4096 bit certificate.
10) If you go the 4096 route, itll prompt you to use a pre generated key. If you're ultra paranoid, press no and wait many hours for it to generate. If you select yes itl will download a random key and use it. 
11) once that's finished, hit enter to use your public IPv4 address as the IP to connect. 
12) select the dns you want to use, (This will be changed to pull from pihole later on)
13) hit the right arrow key and select No, unless you're an advanced user who has a custom domain

And you're done!!! Its a good idea to reboot the pi after installing PiVPN. 

To access the VPN from outside you're network, to do this you'll need to port forward. Every router is different but you'll need to log into the router, find wherever it is (Its called NAT/Gaming on AT&T routers generally).

To create ovpn files, `pivpn add`, enter a name, (its a good practice to create a new profile for every device), press enter when done, set the certificate value to 3650 ( 10 years), then enter a password, then reenter it. It will then generate a key and create the ovpn file. You're ovpn files can be found in `~/ovpns` or `/home/pi/ovpns` that then can be moved out with filezilla or scp. filezilla being the easiest. 

# Configurations

### PiVPN

Once both are installed, you'll want to change PiVPNs DNS server to your pis private IPv4 (usually a 192.168.x.x number)
To do that you'll need to `sudo nano /etc/openvpn/server.conf` and anywhere it say "push-option DNS", add a # at the beginning of the line so it turns blue (if you're using the root user to do everything you wont have the text highlighting) 
![alt-text][ovpnserver]

[ovpnserver]: https://cdn.discordapp.com/attachments/252939120639344640/596850292096761886/5675675757567.png
Something like this
You can either make a new push-option DNS line or edit one of the existing ones (remove the # if you do). And change the IP there to whatever the IP of your pi is (you should have saved this when setting up pihole).
Then once that's edited, Ctrl+X, hit Y, then enter

### PiHole

Now log into the pihole admin webpage.
Go to the Settings tab (on the left) then the DNS tab in the top middle.
1) Change the Interface listening behavior to listen on all interfaces 
2) For a little extra security (this is optional) you can scroll down to the Advanced DNS settings and check the "Use DNSSEC". It helps protect DNS requests from leaking out. 

*Make sure you scroll to the bottom and save your settings*

# Conclusion

Well that's it. Everything should be good to go. With some routers you can set it to force everything on the network to use pihole as a DNS, so there's that option should you want it. The teleporter in the web page is a good thing to remember if you would like to export your custom lists. (To block custom domains from the `pihole -t` or the tail log, you can go to the blacklist tab on the side.)
