# Intro

# Hardware

Raspberry Pi Model B

Sandisk 16 GB SD card, used http://sourceforge.net/projects/win32diskimager/ to transfer ISO to SD card

Wireless: TP-Link TL-WN725N, driver installation instructions at https://www.zhujunsan.net/index.php/2013/03/make-tp-link-tl-wn725n-v2-work-on-raspbian/

https://dl.dropboxusercontent.com/u/1015702/linked_to/2013-06-05%2020.26.44.jpg

# Software

sudo apt-get install mpd mpc

install apache2 php5 libapache2-mod-php5 

cd /var/www, git clone git@github.com:tompreston/MPD-Web-Remote.git

https://dl.dropboxusercontent.com/u/1015702/linked_to/2013-06-16%2018.49.07.png

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

