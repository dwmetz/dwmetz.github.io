---
layout: post
permalink: http://bakerstreetforensics.com/2023/11/09/installing-remnux-on-a-macbook-pro/
title: Installing REMnux on a MacBook Pro
description: None
date: 2023-11-09 20:16:46 -0000
last_modified_at: 2023-11-12 12:09:28 -0000
publish: true
pin: false
categories:
- DFIR
- Forensics
- Mac
- Malware
tags:
- Macbook
- Malware Analysis
- REMnux
- Ubuntu
---
I had an older MacBook Pro (15-inch, 2.53GHz, Mid 2009) that had been unused for a while as it was no longer getting updates from Apple. It's one of the Intel chip ones and last ran Monterey. I pulled it out of the closet and decided to give it a refresh by installing REMnux on it. The process was pretty straightforward, but there were a couple things noted along the way I thought I'd share.

Start off by downloading the [Ubuntu 20.04.6 AMD64 Desktop ISO](https://releases.ubuntu.com/20.04/ubuntu-20.04.6-desktop-amd64.iso). Yes, **20.04**. Later installations aren't supported by the REMnux installer.

Next you'll want to burn the image to a flash drive, and make it bootable, using Rufus (Windows) or Balena Etcher (Mac.) This model MacBook has USB-A ports which seems like a relic compared to the current Macs. You'll need at least an 8GB flash drive for the Ubuntu image. The first free one I could find was 32GB so I used that. 

With the bootable USB drive inserted, power-up the MacBook and hold the **option** key until you see the different hard drives listed.

![](https://bakerstreetforensics.com/wp-content/uploads/2023/11/img_9061.jpeg?w=1024)

The flash drive is the one that shows as **EFI Boot**. Select it and hit return/enter.

![](https://bakerstreetforensics.com/wp-content/uploads/2023/11/img_9062.jpeg?w=1024)

Once everything is booted up you'll get to the _Try or Install Ubuntu_ menu. We’ll choose **install.**

Specify options as needed for timezone, keyboard, etc. For the username we'll use _**remnux**_ and the password **_malware_** as that's the default. After the installation you can set the password for the remnux user as you wish.

At the Installation type we'll choose **Erase disk and install Ubuntu**.

![](https://bakerstreetforensics.com/wp-content/uploads/2023/11/img_9063.jpeg?w=1024)

_Sorry for the wavy resolution. Tough to get good screenshots during bare-metal OS installations._

Once the installation completes, hit **Restart Now**.

When I first logged in I was getting an error, **"Activation of network connection failed"** when trying to authenticate to the wireless network. Disabling IPv6 for that network fixed. it.

![](https://bakerstreetforensics.com/wp-content/uploads/2023/11/ipv6.png?w=1024)

Now that we've got connectivity, we can grab any available Ubuntu updates.
    
    
    sudo apt-get update && sudo apt-get upgrade

![](https://bakerstreetforensics.com/wp-content/uploads/2023/11/apt-update.png?w=1024)

If at any point you're prompted to do a distribution upgrade (a version of Ubuntu later than 20.04), choose **Don't Upgrade**.

![](https://bakerstreetforensics.com/wp-content/uploads/2023/11/no-upgrade.png?w=1024)

Once you've done all the OS updates, and rebooted, we can start the REMnux installation. We'll be following the [Install from Scratch](https://docs.remnux.org/install-distro/install-from-scratch) instructions at remnux.org
    
    
    wget https://REMnux.org/remnux-cli
    sha256sum remnux-cli 

Verify the hash matches the published hash 88cd35b7807fc66ee8b51ee08d0d2518b2329c471b034ee3201e004c655be8d6
    
    
    mv remnux-cli remnux
    chmod +x remnux
    sudo mv remnux /usr/local/bin

The first time I ran the installer it failed as curl wasn't installed. So take care of that before starting the install. 
    
    
    sudo apt-get install curl

![](https://bakerstreetforensics.com/wp-content/uploads/2023/11/curl.png?w=1024)

At this point we're ready to run the installation. The one deviation I'm choosing here is that rather than the standard install, I'm choosing the 'cloud mode.' 

> If you're depoying REMnux in a remote cloud environment and will need to keep the SSH daemon enabled for remotely accessing the system, use the following command instead to avoid disabling the SSH daemon. Remember to harden the system after it installs to avoid unauthorized logins.
> 
> remnux.org

In my case I plan to be ssh'ing into the box from within my own network more often than actual hands on keyboard, hence the cloud mode.
    
    
    sudo remnux install --mode=cloud

![](https://bakerstreetforensics.com/wp-content/uploads/2023/11/install.png?w=1024)

At this point grab a coffee, walk the dog, or find something to do while the wall of text streams by.

_Note if the install fails the first time don't be afraid to re-run the install command a 2nd time._

![](https://bakerstreetforensics.com/wp-content/uploads/2023/11/install-complete.png?w=1024)

Finally when it's done, **Reboot**.

![](https://bakerstreetforensics.com/wp-content/uploads/2023/11/remnux-on-macbook.png?w=1024)

There you go. A shiny, happy, malware analysis machine.
