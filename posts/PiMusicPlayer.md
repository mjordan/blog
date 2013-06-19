# Using a Raspberry Pi as a net radio player

## Intro

This HowTo guide describes setting up a [Raspberry Pi](http://www.raspberrypi.org/) as a net radio player. In my case, playing net radio was about the only thing I couldn't do with my home theatre setup that I wanted to do, so this was a pragmatic use for the cheap, small, simple Linux computer. Basically, my Pi now does the same thing that iTunes does on the desk/laptop, or TuneIn Radio and similar apps do on Android and iOS.

## Hardware

My Raspberry Pi is a Model B. I paid $45 for it from a local vendor. Another $8 bought me a translucent plastic case. The only other hardware I needed to procure were an SD card for the operating system/onboard storage and a USB WiFi adapter. I already had a 16 GB SanDisk card, and I paid $9 for a TP-Link TL-WN725N at the excellent http://ncix.ca. Actually, I also bought a $9 HDMI cable to connect the Pi to my TV (more on that below). Total cost: $71.

The Pi doesn't come with a power supply, but I use the Micro-USB charger from an old LG cellphone. Its output is a standard 5 volt, 700 milliwats.

Setting up the Pi hardware was surprisingly easy. The majority of the setup involved: 1) installing the operating system and 2) installing the the driver for the USB WiFi adapter.

The Pi doesn't come with any onboard storage -- you need to supply the SD card. Installing the operating system is as simple as copying the operating system ISO image onto the SD card. The trick is that you need to use a utility that will copy the ISO image and make the SD card bootable. For the operating system, I chose the standard Raspbian "wheezy" Linux distro, which is available at http://www.raspberrypi.org/downloads. To get it on my 16 GB SanDisk SD card, I used http://sourceforge.net/projects/win32diskimager/, which is extremel easy to use. All you need to do is run the program, select the drive letter of your SD card (my PC has a built-in SD card reader), and then select the ISO file. The utility runs for a minute and then tells you your SD card is ready.

To prepare the Pi for booting, you need to connect it to a monitor (I used the HDMI cable I bought to connect it to my TV), and connect a USB keyboard. For the first boot, I recommend connecting the Pi to your router using an Ethernet cable. The network connection will Just Work and you will be able to download the driver for the USB WiFi adapter.

Once you have the monitor, keyboard, and network cable plugged in, insert the SD card with your OS into the slot on the Pi. Once you've done this, you're ready to turn the Pi on and boot it. There is no power switch; you turn it on by plugging the power supply into the Micro USB connector.

Once it boots, the Pi looks like any other Linux computer. The first time it boots, a simple configuration menu appears that lets you change the password for the default user (username pi), etc.

The Pi doesn't come with an onboard wireless adapter. However, it does come with two USB ports. Many USB WiFi adapters work with Raspbian; I chose a TP-Link TL-WN725N because it is very small (smaller than a fingernail) and cheap ($9, you may pay less). To use this adapter you will need to install a driver for it. Installation instructions are at https://www.zhujunsan.net/index.php/2013/03/make-tp-link-tl-wn725n-v2-work-on-raspbian/. The instructions on this page worked without a glitch, and the wireless adapter works reliably reboot after reboot.

One final step: I wanted to assign my Pi a stable IP address, so I needed to configure my router (a Linksys ERT54G running Tomato) to assign the Pi's MAC address a static IP. Unfortunately, the WiFi adapter's MAC address is not printed anywhere on its packaging, so I needed to get it working and then run ifconfig to get its hardware address, which I registered in my router's static DHCP configuration. Your Pi doesn't need a static IP address to work, but if you want to use your phone or other device with a web browser as a remote, it's convenient to know what IP address to use.

Here is a picture of the Pi without the DDMI cable attaching it to the TV:

![The Pi](https://dl.dropboxusercontent.com/u/1015702/linked_to/2013-06-05%2020.26.44.jpg)

## Software

Apart from the operating system, you will want to install mpd (Music Player Daemon) and its standard client, mpc. There are many clients for mpd, for all platforms. I chose to install a web interface to mpd so I could control my player from a variety of devices. After trying a couple I chose MPD Web Remote (github.com:tompreston/MPD-Web-Remote) because it looked good and was so easy to set up.

Here are the commands you need to run to install all of the software mentioned in the previous paragraph (Apache and PHP are required by MPD Web Remote):

```
sudo apt-get install mpd mpc
sudo apt-get apache2 php5 libapache2-mod-php5 
cd /var/www
sudo git clone git@github.com:tompreston/MPD-Web-Remote.git
```
That's it. MPD Web Remote requires zero configuration. Any WebKit browser will work (which means Chrome desktop and mobile, Safari, and the browser on Blackberry phones), but Firefox and IE don't work very well. Here's a screencap of MPD Web Remote on my Android phone running Chrome:

![MPD Web Remote in Chrome for Android](https://dl.dropboxusercontent.com/u/1015702/linked_to/mpd-web-client.jpg)

## Playlists

The hardest part of getting the Pi working as a net radio player is creating playlists.

Look inside .pls files for URLs

/var/lib/mpd/playlists

```
#EXTM3U
#EXTINF:-1,CBC Radio 1 Vancouver
http://1671.live.streamtheworld.com:80/CBC_R1_VCR_L_SC
#EXTINF:-1,CBC Radio 2 Vancouver
http://1671.live.streamtheworld.com:80/CBC_R2_EDM_H_SC
```

```
#EXTM3U
#EXTINF:-1,WGBO Jazz
http://wbgo.streamguys.net/wbgo96
```

```
#EXTM3U
#EXTINF:-1,Groove Salad
http://mp3.somafm.com:80
#EXTINF:-1,Mission Control
http://mp1.somafm.com:2020
#EXTINF:-1,Space Station Soma
http://mp2.somafm.com:2666
```

## Connecting the Pi to my home theatre receiver

