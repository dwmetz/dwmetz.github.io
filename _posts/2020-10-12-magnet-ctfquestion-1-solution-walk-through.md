---
layout: post
permalink: http://bakerstreetforensics.com/2020/10/12/magnet-ctfquestion-1-solution-walk-through/
title: 'Magnet CTF:

  Question 1 Solution Walk-Through'
description: None
date: 2020-10-12 15:13:12 -0000
last_modified_at: 2020-10-12 15:13:12 -0000
publish: true
pin: false
categories:
- CTF
- DFIR
tags:
- Magnet; DFIR; Forensics; CTF; Android
---
_**1: What time was the file that maps names to IP's recently accessed?**_ ![UntitledImage](http://bakerstreetforensics.com/wp-content/uploads/2020/10/android.png)

Mobile Forensics is not my strongest area, and Android even less than iOS. Based on my limited experience the first thing I started with was Google (“GTS”). Based on the question I supposed that the artifact would be DNS related. Where on the device would that be set locally? To my delight I learned that on Android there is a local **hosts** file that is responsible for mapping IP’s to DNS (what do you know just like Windows and Linux).

Doing a Global Search for **hosts** there are a number of hits, but nothing for the **hosts** file itself.

![UntitledImage](http://bakerstreetforensics.com/wp-content/uploads/2020/10/untitledimage.png)

The first time I processed the Android image tar file I did it as

**Mobile > Android > Load Evidence > Image**

Using this format when I went to the file explorer view in Magnet, all that was visible was the tar file and I couldn’t navigate the directory structure.

I extracted the tar file (using 7zip) and then re-processed the evidence as

**Mobile > Android > Load Evidence > Files and ****Folders**

This yielded the same number of artifacts; however, it exposed the directory structure for browsing in **File System** view.

![UntitledImage](http://bakerstreetforensics.com/wp-content/uploads/2020/10/untitledimage-1.png)

In the **File System** view we can now run a search for **hosts** (be sure to enable subdirectory results if you’re not focusing on a particular path.

![UntitledImage](http://bakerstreetforensics.com/wp-content/uploads/2020/10/untitledimage-2.png)

In this case the **hosts** file can be found at /data/adb/modules/hosts/system/etc

![UntitledImage](http://bakerstreetforensics.com/wp-content/uploads/2020/10/untitledimage-3.png)

Looking at the preview we can see an additional entry for **malliesae.com**

![UntitledImage](http://bakerstreetforensics.com/wp-content/uploads/2020/10/untitledimage-4.png)

With the hosts file selected, scrolling to the right reveals the Created, Accessed and Modified times for this file. Here we see that the file was modified **03/05/2020****05:50:18.**

**PDF:[MagnetWeeklyCTF Write-Up 1.pdf](http://bakerstreetforensics.com/wp-content/uploads/2020/10/magnetweeklyctf-write-up-1.pdf "MagnetWeeklyCTF Write-Up 1.pdf")**
