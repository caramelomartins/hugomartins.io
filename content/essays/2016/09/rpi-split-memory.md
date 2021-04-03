---
title: GPU Memory Splitting on Raspbian Jessie
date: "2016-09-05T00:00:00"
aliases: ["/blog/gpu-memory-splitting-on-raspbian-jessie/"]
category:
  - helpers
  - raspberry pi
---

Lately, I've running a little side project with the aim of setting up a self-hosted array of software in one my old, beat up laptops.

While I was doing this, I thought that it would be as useful and interesting to try and set up a my personal home theatre - I chose [Kodi](https://kodi.tv/) for the job.

Even though it only supports 15.2 [officially](http://kodi.wiki/view/HOW-TO:Install_Kodi_on_Raspberry_Pi) on Raspbian, there's an [unofficial](https://mtantawy.com/quick-tip-how-to-update-to-latest-kodi-16-jarvis-on-raspberry-pi/) repository using 16.1 which means that I could use the full power of Kodi on a low power hardware such as RPi 2.

I started using it and unfortunately it always seem to break when I was trying to stream videos. I couldn't understand why. I changed Skins, Add-ons, streaming sources and what not. I couldn't figure out why, some videos broke and some didn't.

Off to Google I went.

It took me about 10 to 15 minutes to understand what might be the problem. GPU processing was being streched to its maximum capabilities.

On the Raspberry [forums](https://www.raspberrypi.org/forums/viewtopic.php?f=91&t=58245) I was able to understand that GPU doesn't have it's own RAM, and that GPU is responsible for the graphics processing involved in video playback and streaming.

I thought to myself that if I tried to watch something lower than HD...maybe it'd work.

I turned on Kodi and started testing. Lower than HD video...worked like a charm. Higher than HD video...crash.

After the problem origin was found, it was time to understand the solution which turned around GPU RAM I was sure by now.

After googling some more, I found that it was possible to configure Raspbian memory splitting directly from `raspi-config`:

`sudo raspi-config`

Select _Advanced Options_:

![raspbian-advanced-configurations](/images/1473110638.png)

Select _A3 Memory Split_:

![raspbian-memory-split](/images/1473110890.png)

Configure depending on your needs. I went with _265_:

![raspbian-memory-values](/images/1473111017.png)

Select _OK_. Select _Finish_ and reboot your Raspberry Pi.

This worked for me and helped me achieve better graphics performance on Raspbian.
