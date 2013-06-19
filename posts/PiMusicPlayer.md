# Intro

This HowTo guide describes setting up a [Raspberry Pi](http://www.raspberrypi.org/) as a net radio player. In my case, playing net radio was about the only thing I couldn't do with my home theatre setup that I wanted to do, so this was a pragmatic use for the cheap, small, simple Linux computer. Basically, my Pi now does the same thing that iTunes does on the desk/laptop, or TuneIn Radio and similar apps do on Android and iOS.

# Hardware

My Raspberry Pi is a Model B. I paid $45 for it from a local vendor. Another $8 bought me a translucent plastic case. The only other hardware I needed to procure were an SD card for the operating system/onboard storage and a USB WiFi adapter. I already had a 16 GB SanDisk card, and I paid $9 for a TP-Link TL-WN725N at the excellent http://ncix.ca. Actually, I also bought a $9 HDMI cable to connect the Pi to my TV (more on that below). Total cost: $71.

The Pi doesn't come with a power supply, but I use the Micro-USB charger from an old LG cellphone. Its output is a standard 5 volt, 700 milliwats.

Setting up the Pi hardware was surprisingly easy. Literally two steps: 1) install the operating system and 2) install the USB WiFi adapter.

1) Sandisk 16 GB SD card, used http://sourceforge.net/projects/win32diskimager/ to transfer ISO to SD card

Boot (i.e., plug it in), then follow the setup menu. 

2) TP-Link TL-WN725N, driver installation instructions at https://www.zhujunsan.net/index.php/2013/03/make-tp-link-tl-wn725n-v2-work-on-raspbian/

![The Pi](https://dl.dropboxusercontent.com/u/1015702/linked_to/2013-06-05%2020.26.44.jpg)

# Software

Here are the steps required to install MPD (), MPC, and a web interface to MPD -- I chose MPD Web Remote (github.com:tompreston/MPD-Web-Remote).

sudo apt-get install mpd mpc
sudo apt-get apache2 php5 libapache2-mod-php5 
cd /var/www
sudo git clone git@github.com:tompreston/MPD-Web-Remote.git

![MPD Web Remote in Chrome for Android](https://dl.dropboxusercontent.com/u/1015702/linked_to/2013-06-16%2018.49.07.png)

# Playlists

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

# Connecting the Pi to my home theatre receiver

