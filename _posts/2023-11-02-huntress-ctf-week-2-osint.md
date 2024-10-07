---
layout: post
permalink: http://bakerstreetforensics.com/2023/11/02/huntress-ctf-week-2-osint/
title: 'Huntress CTF: Week 2 - OSINT: Where Am I?, Operation Not Found, Under the
  Bridge'
description: None
date: 2023-11-02 15:00:00 -0000
last_modified_at: 2023-10-22 11:16:24 -0000
publish: true
pin: false
categories:
- CTF
- DFIR
- PowerShell
tags:
- exiftool
- HuntressCTF
- OSINT
---
## Where Am I?

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-09-at-9.53.49e280afam.png?w=1024)

Opening the picture we see it's a location.

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/pxl_20230922_231845140_2.jpg?w=768)

I've frequently used [exiftool](https://exiftool.org) to inspect the metadata of pictures, including GPS coordinates. 

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-09-at-10.01.24e280afam.png?w=1024)

The file does contain GPS metadata but before we even get there, looks like something out of the ordinary for the Image Description...

Instead of the usual CyberChef, this time we'll do the conversion using PowerShell.

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-09-at-10.05.24e280afam.png?w=1024)

The converted string is our flag.

* * *

## Operation Not Found

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-12-at-7.06.22e280afam.png?w=1024) ![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-17-at-12.11.42e280afpm.png?w=1024)

First off, lets adjust the positioning of the image and see if we can get better view of our location.

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-17-at-12.12.09e280afpm.png?w=1024)

That's better.

Actually when I ran this challenge, I started on my mobile device.

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/img_8886.png?w=472)

I took a screenshot of the building and then used the **Google Lens** function to identify the building.

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/img_8887.png?w=472)

Georgia Tech Library. That's consistent with the description in the challenge. I bring up the location in Google Maps.

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-12-at-7.10.42e280afam.png?w=1024)

Zooming and scrolling and zooming and scrolling to get the Google Maps location and the mini-map on the challenge to the same areas. The mini-map is a PAIN to navigate. Even knowing where I was going to it took me several minutes to manipulate my positioning on the map.

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-12-at-7.12.30e280afam.png?w=1024)

But once I'm finally there, I mark my location and submit for the flag and...

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-12-at-7.24.16e280afam.png?w=1024)

* * *

## Under the Bridge

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-13-at-9.01.36e280afam.png?w=1024)

Pretty much the same methodology as above. 

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/img_8893.png?w=472)

Pivot the screen for a clearer landmark.

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/img_8894.png?w=472)

Grab a screenshot and send it to Google Lens

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/img_8897.png?w=472)

Rickroll Tunnel. LOL.

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/img_8898.jpeg?w=601) ![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/img_8895.png?w=472)

Once again back and forth with Google Maps and the mini-map and getting familiar with all the London highways, and finally....

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-13-at-9.28.57e280afam.png?w=1024)

* * *

Use the tag [**#HuntressCTF**](https://bakerstreetforensics.com/tag/HuntressCTF/) on BakerStreetForensics.com to see all related posts and solutions for the 2023 Huntress CTF.

* * *
