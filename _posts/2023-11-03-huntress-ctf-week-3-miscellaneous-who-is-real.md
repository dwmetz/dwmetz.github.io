---
layout: post
permalink: http://bakerstreetforensics.com/2023/11/03/huntress-ctf-week-3-miscellaneous-who-is-real/
title: 'Huntress CTF: Week 3 - Miscellaneous: Who Is Real?, Operation Eradication'
description: None
date: 2023-11-03 17:00:00 -0000
last_modified_at: 2023-10-27 16:07:27 -0000
publish: true
pin: false
categories:
- CTF
- DFIR
- Miscellaneous
tags:
- AI
- exfiltration
- HuntressCTF
- rclone
---
## Who Is Real?

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-19-at-9.33.46e280afam.png?w=1024)

This was a change of pace from what a lot of the CTF has been; lots of malware and deobfuscation. In this challenge we're tasked with figuring out which faces are real and which have been AI generated.

Before starting the challenge, I familiarized myself with

https://whichfaceisreal.com/learn.html

It gave me good ideas of things to look for regarding teeth, glasses, earrings, other faces in photos, etc.

_Eventually_ , I was able to get 5 right in a row.

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-19-at-9.23.15e280afam.png?w=1024)

* * *

## Operation Eradication

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-20-at-9.27.04e280afam.png?w=1024)

Let's take a look at the configuration file.

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-27-at-11.37.03e280afam.png?w=675)

This looks like a config file for **rclone**.

Using this information, and the url provided from the challenge, we can update our rclone config file.

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-27-at-11.41.23e280afam.png?w=948)

Now using rclone we can connect to the remote location and hopefully start deleting these 'sensitive' files. If only it were so easy.

I was able to get a directory listing, so I knew that my credentials were successfully connecting.

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-27-at-11.49.54e280afam.png?w=1024)

I was all over the command options at <https://rclone.org>. Every DELETE or SYNC operation I could think of was failing. 

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-21-at-10.05.44e280afam.png?w=1024)

At my wit's end I pinged a friend who suggested trying to overwrite the files with a 0 bit file. If successful the files would still be there, but the content gone - so essentially, they'd be _safe_ again.

Using the file listing from the server, I wrote a PowerShell script that would touch, or create a 0 byte file, locally for each file names.

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-27-at-11.57.32e280afam.png?w=1024)

Next the script would run the rclone copy command to copy the local 0 byte files to the network location.

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-27-at-11.57.58e280afam.png?w=1024)

I run the PowerShell script and then return to the webpage and refresh...

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-24-at-8.16.40e280afam.png?w=1024)

DOH! There was a typo in one line of the script. I'll re-run the file listing command again. All but one file have the 0 byte file size.

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-24-at-8.49.39e280afam.png?w=1024)

Run the copy command one more time to take care of our errant file and...

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-24-at-8.24.51e280afam.png?w=1024)

Success!

* * *

Use the tag [**#HuntressCTF**](https://bakerstreetforensics.com/tag/HuntressCTF/) on BakerStreetForensics.com to see all related posts and solutions for the 2023 Huntress CTF.
