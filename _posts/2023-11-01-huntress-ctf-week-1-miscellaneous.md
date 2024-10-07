---
layout: post
permalink: http://bakerstreetforensics.com/2023/11/01/huntress-ctf-week-1-miscellaneous/
title: 'Huntress CTF: Week 1 - Miscellaneous: I Won''t Let You Down'
description: None
date: 2023-11-01 15:00:00 -0000
last_modified_at: 2023-10-22 11:14:11 -0000
publish: true
pin: false
categories:
- CTF
- DFIR
tags:
- HuntressCTF
- nmap
- rickroll
---
## I Won't Let You Down

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-05-at-10.54.35e280afam.png?w=867)

If you went to the web page in a browser, there was a suggestion to use **nmap**. There was also an embedded video of Rick Astley. 

[Nmap](https://nmap.org) is a tool I've used over and over in my career. I may have even had Nmap Ninja on my resume or LinkedIn at a time. I always get a kick out of seeing it used in [movies](https://nmap.org/movies/), and it's be used in a lot.

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-05-at-10.06.52e280afam.png?w=703)

A basic, albeit thorough nmap command gives us:

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-05-at-10.07.34e280afam.png?w=1024)

Ok, so let's start knocking on ports.

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-09-at-10.46.57e280afam.png?w=1024)

It's not SSH. What about the other ports? When we telnet to port 8888 we get...

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-05-at-10.40.43e280afam.png?w=803) ![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/2c9427649bc8bf67819a62964cb7adab-2902903957-2.jpeg?w=1024) ![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-05-at-10.08.05e280afam.png?w=681)

* * *

Use the tag [**#HuntressCTF**](https://bakerstreetforensics.com/tag/HuntressCTF/) on BakerStreetForensics.com to see all related posts and solutions for the 2023 Huntress CTF.
