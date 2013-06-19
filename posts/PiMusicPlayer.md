# Using a Raspberry Pi as a net radio player

## Intro

This HowTo guide describes setting up a [Raspberry Pi](http://www.raspberrypi.org/) as a net radio player. Playing net radio was about the only thing I couldn't do with my home theatre syatem that I wanted to do, so this was a pragmatic use for the cheap, small, simple Linux computer. Basically, my Pi now does the same thing that iTunes does on the desk/laptop, or TuneIn Radio and similar apps do on Android and iOS.

## Hardware

My Raspberry Pi is a Model B. I paid $45 for it from a local vendor (http://www.leeselectronic.com/). Another $8 bought me a translucent plastic case. The only other hardware I needed to procure were an SD card for the operating system/onboard storage and a USB WiFi adapter. I already had a 16 GB SanDisk card, and I paid $9 for a TP-Link TL-WN725N at http://ncix.ca. Actually, I also bought a $9 HDMI cable to connect the Pi to my TV (more on that below). Total cost: $71.

The Pi doesn't come with a power supply, but I use the Micro-USB charger from an old LG cellphone. Its output is a standard 5 volt, 700mA. An excellent opportunity to reuse something laying around the house.

Here is a picture of the Pi without the HDMI cable attaching it to the TV, and without the USB keyboard connected:

![The Pi](https://dl.dropboxusercontent.com/u/1015702/linked_to/2013-06-05%2020.26.44.jpg)

Setting up the Pi hardware was surprisingly easy. The majority of the setup involved: 1) installing the operating system and 2) installing the the driver for the USB WiFi adapter.

The Pi doesn't come with any onboard storage -- you need to supply the SD card. Installing the operating system is as simple as copying the operating system ISO image onto the SD card. The trick is that you need to use a utility that will copy the ISO image and make the SD card bootable. For the operating system, I chose the standard Raspbian "wheezy" Linux distro, which is available at http://www.raspberrypi.org/downloads. To get it on my 16 GB SanDisk SD card, I used http://sourceforge.net/projects/win32diskimager/, which is extremel easy to use. All you need to do is run the program, select the drive letter of your SD card (my PC has a built-in SD card reader), and then select the ISO file. The utility runs for a minute and then tells you your SD card is ready. Similar utilities exist for Linux (e.g., dd) and OSX (e.g., PiWriter).

To prepare the Pi for booting, you need to connect it to a monitor (I used the HDMI cable I bought to connect it to my TV), and connect a USB keyboard. For the first boot, I recommend connecting the Pi to your router using an Ethernet cable. The network connection will Just Work and you will be able to download the driver for the USB WiFi adapter (link to instructions below).

Once you have the monitor, keyboard, and network cable plugged in, insert the SD card with your OS into the slot on the Pi. Once you've done this, you're ready to boot it. There is no power switch; you boot it by plugging the power supply into the Micro USB connector.

The first time the Pi boots, a simple configuration menu appears that lets you change the password for the default user (username pi), etc. When you exit the configuration menu, you arrive at a command shell. If you want to see the Pi's graphical user interface, run 'startx' (but you don't need to start the GUI if you just want to install the net radio software).

The Pi doesn't come with an onboard wireless adapter. However, it does come with two USB ports. Many USB WiFi adapters work with Raspbian; I chose a TP-Link TL-WN725N because it is very tiny (smaller than a finger nail) and cheap ($9, you may pay less). To use this adapter you will need to install a driver for it. Installation instructions are at https://www.zhujunsan.net/index.php/2013/03/make-tp-link-tl-wn725n-v2-work-on-raspbian/. The instructions on this page worked without a glitch, and the wireless adapter works reliably reboot after reboot. To make the wireless interface the default (as opposed to the wired network connection you used earlier), you will need to edit /etc/network/interfaces, and if your wireless network requires authentication like mine does, you will need to add your WPA (or whatever) password to this file as well. My /etc/network/interfaces file looks like this:

```
auto lo

iface lo inet loopback
#iface eth0 inet dhcp

allow-hotplug wlan0
auto wlan0
iface wlan0 inet dhcp
        wpa-ssid "myssid"
        wpa-psk "mypassword"
iface default inet dhcp
```

One final step: I wanted to assign my Pi a stable IP address, so I needed to configure my router (a Linksys ERT54G running Tomato) to assign the Pi's MAC address a static IP. Unfortunately, the WiFi adapter's MAC address is not printed anywhere on its packaging, so I needed to get it working and then run ifconfig to get its hardware address, which I then registered in my router's static DHCP configuration. Your Pi doesn't need a static IP address to get the music to play, but if you want to use your phone or other device with a web browser as a remote, it's convenient to know what IP address to use.

## Software

Apart from the operating system, you will want to install mpd (Music Player Daemon) and its standard client, mpc. There are many clients for mpd, for all platforms. I chose to install a web interface to mpd so I could control my player from my phone, tablet, and laptop web browser. After trying a couple I chose MPD Web Remote (github.com:tompreston/MPD-Web-Remote) because it looked good and was so easy to set up.

Here are the commands you need to run to install all of the software mentioned in the previous paragraph (Apache and PHP are required by MPD Web Remote):

```
sudo apt-get install mpd mpc
sudo apt-get apache2 php5 libapache2-mod-php5 
cd /var/www
sudo git clone git@github.com:tompreston/MPD-Web-Remote.git
sudo mv MPD-Web-Remote music
```
That's it. MPD Web Remote requires zero configuration. Any WebKit browser will work (which means Chrome desktop and mobile, Safari, and the browser on Blackberry phones), but Firefox and IE don't work very well. Here's a screencap of MPD Web Remote on my Android phone running Chrome:

![MPD Web Remote in Chrome for Android](https://dl.dropboxusercontent.com/u/1015702/linked_to/mpd-web-client.jpg)

## Playlists

The hardest part of getting the Pi working as a net radio player is creating playlists. Playlists group together your net radio stations' streaming URLs so they are easier to browse in whatever client you are using, and also let you assign labels to the streams. To create the playlists, you need to ssh into your Pi and create a .m3u file for each playlist. By default, these files live in /var/lib/mpd/playlists on the Pi but this location is configurable in /etc/mdp.conf. Here are three playlist files I use, named CBC.m3u, Jazz.m3u, and SomaFM.m3u respectively:

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
It's not always easy to know which URLs to use for the streams. Some stations, like Soma FM, provide their stream URLs very openly, while others, like CBC, make you work. A useful trick is to look inside playlist files that end in .pls; these usually contain the direct streaming URLs that you want to include in your .m3u playlists used by mpd.

mpd doesn't provide the slick browsability that TuneIn or iTunes do. You need to ssh into your Pi and maintain your playlists using a text editor. mpd is a complex and powerful program and you can waste many hours playing with its options. However, the sample playlists above, and a decent client like MPD Web Remote are all you need to get going.

Once you have some playlists, point your browser at your MPD-Web-Remote's URL and you should see something similar to the image above.

## Connecting the Pi to my home theatre receiver

Actually, the biggest challenge in gettin my Pi to play through my home theatre system was connecting it to the receiver. The Pi has two audio outputs, a 3.5 mm headphone-style jack and the HDMI output, which supports digital audio. Skip the 3.5 mm output, as it apparently provides very poor-quality sound (it outputs 11-bit sound, which according to any gearhead you ask is far inferior to the 16-bit sound that music CDs produce).

My problem was that my reciever is old and doesn't have any HDMI inputs. Not only that, all of my digital audio inputs are in use. The only solution I was able to come up with was to route the HDMI output from the Pi into my TV (which has an HDMI audio input), then to run RCA cables from an audio output on the TV into the receiver. Not optimal but still better than the poor sound produced by the 3.5 mm output on the Pi.

Happy listening.

