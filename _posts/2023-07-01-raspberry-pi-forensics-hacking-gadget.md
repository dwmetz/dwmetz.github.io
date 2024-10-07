---
layout: post
permalink: http://bakerstreetforensics.com/2023/07/01/raspberry-pi-forensics-hacking-gadget/
title: Raspberry Pi Forensics Hacking Gadget
description: None
date: 2023-07-01 16:30:18 -0000
last_modified_at: 2023-07-02 11:21:08 -0000
publish: true
pin: false
image:
  path: https://bakerstreetforensics.com/wp-content/uploads/2023/07/img_8340.jpg
categories:
- DFIR
- DIY
- Raspberry Pi
- USB
tags:
- Forensics
- Linux
---
Ever since the 2021 iPad models with USB-C chargers came out, I’ve been intrigued by the notion of Raspberry Pi gadgets. In short, these are Raspberry Pi devices that draw their power, and/or networking from the USB-C port on the iPad Pro. 

Having awakened my tinkering spirit with the [internet speed monitor](https://bakerstreetforensics.com/2023/06/12/raspberry-pi-internet-speed-monitor/) project, I was looking for another project. I had one unused Raspberry Pi Zero W in a box of spare Pi parts, so that’s where I started. 

I chose Kali for the distribution to use because there are images specific to various Raspberry Pi hardware models, and because the distribution itself supports many popular Linux tools for Forensics and Reverse Engineering. REMnux is my default Linux for malware poking, but to date it’s only supported on Intel architectures. 

Know from the start you’re not going to be using this device for processing on the scale of Enron, but for access to a wider toolset when on the go, and especially for training I think it’s a pretty cool setup. If you’re looking to set up a mobile development environment, or still run Kali but with more _oomf_ \- there’s number of resources to do so using a Pi 4. Since the Pi Zero W is powered by a USB-micro, it cannot support networking (iPad to Pi) over the USB port. Later models like the Pi 4 (USB-C powered) are capable, but at the time of the project, all mine be were occupied. In this case we’ll be connecting to the Pi over WiFi via SSH. 

Grab the image for Pi Zero W (or whatever’s applicable for the model you’re running from <https://www.kali.org/get-kali/#kali-arm>. There’s plenty of documentation on enabling SSH if it isn’t by default. On this particular build for the Pi, it was. You’ll also want to install tightvncserver.

![](https://bakerstreetforensics.com/wp-content/uploads/2023/07/img_0589.jpg?w=1024)

Depending on which Pi hardware version you’re using, the Pi will have different capabilities. Notably lacking on the Pi Zero W, the resources to run any modern browser. But since I have the iPad that it’s running from it’s not like I’m missing it at all.

Kali supports the installation of what they call meta-packages. These are specific sets of tools or features to support different capabilities (Bluetooth hacking, wireless hacking, etc.) For my build I chose the reverse engineering and forensics packages as those are the tools I’m most interested in experimenting with.

![](https://bakerstreetforensics.com/wp-content/uploads/2023/07/img_0590.jpg?w=1024) ![](https://bakerstreetforensics.com/wp-content/uploads/2023/07/img_0591.jpg?w=1024)

I had a bit of trial and error when it came to the physical USB connections. Originally I had a series of USB-C connecting adapters, terminating with a USB-C to USB micro adapter. When I had this franken-jack plugged into the iPad the Pi wouldn’t power up. However if I had a USC-C cable connected to the jack, or between the jack and the iPad, I could get power (just with a cable I didn’t need.) At some point I had the idea of introducing a USB-A into the mix and voila, power to the Pi. All that said, the final hardware combo consisted of a USB-C (male) to USB-A (female) 180 degree adapter, and a USB-A (male) to USB-Micro (male) adapter. 

![](https://bakerstreetforensics.com/wp-content/uploads/2023/07/img_8340.jpg?w=1024) ![](https://bakerstreetforensics.com/wp-content/uploads/2023/07/img_8341.jpg?w=1024) ![](https://bakerstreetforensics.com/wp-content/uploads/2023/07/img_8342.jpg?w=498)

The 180 degree adapter enables a very low profile while having a reasonable gap for ventilation, even when connected to a Magic Keyboard.

Plug the device into the USB-C port on the iPad a give it a minute or two to boot up. 

For SSH on the iPad there’s no better than [Blink](https://apps.apple.com/us/app/blink-shell-build-code/id1594898306). 

I don’t have VNC running at boot to save on resources, but I have a script in my home directory to quickly turn it on when GUI access is needed. 

For VNC I use[ Jump Desktop](https://apps.apple.com/us/app/jump-desktop-rdp-vnc-fluid/id364876095), and have a configuration saved for VNC tunneled over SSH. 

![](https://bakerstreetforensics.com/wp-content/uploads/2023/07/img_0588.jpg?w=1024) ![](https://bakerstreetforensics.com/wp-content/uploads/2023/07/img_0587.jpg?w=1024)
