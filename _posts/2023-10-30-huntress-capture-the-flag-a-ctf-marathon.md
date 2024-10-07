---
layout: post
permalink: http://bakerstreetforensics.com/2023/10/30/huntress-capture-the-flag-a-ctf-marathon/
title: Huntress Capture the Flag - A CTF Marathon
description: None
date: 2023-10-30 15:55:20 -0000
last_modified_at: 2023-10-30 19:12:04 -0000
publish: true
pin: false
categories:
- CTF
- DFIR
- Malware
- Memory Analysis
- OSINT
- Steganography
tags:
- Forensics
- HuntressCTF
- Malware Analysis
---
![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-05-at-1.06.05e280afpm-1.png?w=1000)

Throughout October, as part of Cyber Security Awareness Month, the team over at Huntress put on a ~30 day Capture the Flag event with 58 unique challenges. 

First and foremost, kudos to the organizers for pulling off an event of this size and duration. There were only minor technical difficulties noticed throughout the month, and on more than one occasion those were due to people not observing the rules and using brute force tools where they weren't needed (or allowed.) 

Overall, I found the event to be a great learning experience that challenged me, increased my confidence, and gave me an avenue to pursue skills I want to develop further.

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/meme-trendmicro-cybersecurity-socialmedia-cybercrime-infosec-malware-spyware-ransomware-virus-twitter-cyberwar-databreach-784x348-1463541974.gif?w=784)

The challenges covered a wide area of subjects with the majority being DFIR related. The categories included:

  * Warm Ups (14)
  * Forensics (10)
  * Malware (16)
  * M365 (4)
  * OSINT (3)
  * Steganography (1)
  * Miscellaneous (10)



Today the final challenge of the event, graced us with another lovely malware sample to analyze.

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/malware_is_coming_got-999246027.jpeg?w=603)

I was very pleased with myself at having solved nearly 80% of the challenges. _There's still another 20 or so hours to go, so we'll see if that improves any further._ The only categories I didn't have 100% in were Miscellaneous and Malware. I think this is fair considering my skill levels. The Malware scenarios were appropriately challenging for someone newer to this area. This is an area that I've been developing my skills in more recently. I'm looking forward to seeing others' write-ups on those challenges where I didn't make it all the way through, and following along with my own data.

## Tools used in the CTF

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/static-malware-analysis-tools-2716113288-1.png?w=928)

I added a number of new tools to my toolkit throughout the CTF, and got more experienced with some old friends as well. Depending on the challenge I switched between operating systems including MacOS, REMnux (Linux), and a customized Windows VM with a plethora of malware analysis utilities. By the end of the event the tools used included:

  * PowerShell
  * Strings
  * CyberChef
  * Gimp
  * Curl
  * Firepwd.py
  * rita
  * the_silver_searcher
  * nmap
  * dcode.fr
  * meld
  * Cutter
  * Ghidra
  * Python
  * ChatGPT
  * Google Chrome Developer Tools
  * iSteg
  * exiftool
  * Google Lens
  * Google Maps
  * detonaRE
  * Process Monitor (procmon)
  * Visual Studio Code
  * Site Sucker
  * 7zip
  * Magnet AXIOM
  * olevba
  * x64dbg
  * AADInternals
  * Microsoft Excel
  * Event Viewer
  * chainsaw
  * PowerDecode
  * PowerShell ISE
  * rclone
  * Volatility3
  * hashcat
  * impacket



## Write-Ups

Over the next few days I'll be releasing the write-ups on how I solved each of the completed challenges. _The organizers requested that no solutions be posted until 24 hours after the conclusion of the CTF._

Based on the amount of content, I'll be breaking the write-ups down by week number (1-4) and challenge category.

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/image-1.jpeg?w=1024)

**_Wednesday:_**

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-30-at-10.29.22e280afam.png?w=904)

**_Thursday:_**

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-30-at-10.30.00e280afam.png?w=903)

**_Friday:_**

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-30-at-10.30.40e280afam.png?w=901)

**_Saturday:_**

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-30-at-10.31.08e280afam.png?w=901)

You can follow along through the week, or come back on the weekend to read them all.

Once again, I want to extend my thanks to the Huntress team for a great event. I hope you'll follow along with my solutions, and please comment with other ways to solve if you have them. It's all about the learning.

* * *

Use the tag **[#HuntressCTF](https://bakerstreetforensics.com/tag/HuntressCTF/)** on BakerStreetForensics.com to see all related posts and solutions for the 2023 Huntress CTF.
