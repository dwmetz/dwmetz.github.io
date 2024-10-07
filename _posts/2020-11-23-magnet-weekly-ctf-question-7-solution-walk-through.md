---
layout: post
permalink: http://bakerstreetforensics.com/2020/11/23/magnet-weekly-ctf-question-7-solution-walk-through/
title: 'Magnet CTF: Question 7 Solution Walk-Through'
description: None
date: 2020-11-23 16:02:00 -0000
last_modified_at: 2020-11-30 02:31:30 -0000
publish: true
pin: false
categories:
- CTF
- DFIR
tags:
- Magnet; DFIR; Forensics; CTF; Hadoop; Linux
---
Unlike the Fighting Irish, I don't have a perfect record this year - but I'm still loving the game. I never did get to finish the week 6 challenge, but with week 7, I'm back in it.

_**Challenge 7 (Nov 16-23) Part 1, Domains and Such. What is the IP address of the HDFS primary node?**_

As I was gathering information about Linux forensics, I came across **LinuxForensicsForNon-LinuxFolks** from Hal Pomeranz. (Google it). It's chock full of pointers on where to find particular artifacts as the correspond to their Windows counterparts, and as the title indicates it's meant for novices to Linux.

To identify the IP address of a Linux host there are a few places to check. If the address is assigned statically it will be in **/etc/hosts**. If the address is assigned by DHCP there should be a reference in **/var/lib/dhclient** and/or **/var/log/***.

Bringing up our evidence in the File System view in Magnet Axiom, we navigate to **/etc/hosts**.

![](https://bakerstreetforensics.com/wp-content/uploads/2020/11/hosts.png?w=1024)

We can see that the primary node has the IP assignment of **192.168.2.100**. [Flag 1]

After not being able to finish the challenge the week before, I was so excited to get the flag I nearly (or did) miss the fact that this was a 3-part question. It was only later in the afternoon watching the video introducing the challenge that I realized it had multipe sections. 

**_Part 2: Is the IP address on HDFS-Primary dynamically or statically assigned?_**

Based on the fact that the address was _in_ the hosts file, that indicates that the address was assigned **statically.** [Flag 2]

**_Part 3: What is the interface name for the primary HDFS node?_**

For this answer we navigate to **/etc/network/interfaces.**

![](https://bakerstreetforensics.com/wp-content/uploads/2020/11/interfaces.png?w=1024)

Previewing the content of the file we see that **ens33** [Flag 3] is the name of the interface. Had we identified this file first we would have been able to surmise all 3 flags from the same source. As with all things forensics, there are many ways to get to the same information. Understanding how those compare and what the outliers are, is all part of the challenge.

That's all for this week. Now I'm off to watch the next video so I can see what I missed in last weeks challenge.
