---
layout: post
permalink: http://bakerstreetforensics.com/2023/11/04/huntress-ctf-week-4-forensics-bad-memory/
title: 'Huntress CTF: Week 4 - Forensics: Bad Memory'
description: None
date: 2023-11-04 13:00:00 -0000
last_modified_at: 2023-10-24 20:19:44 -0000
publish: true
pin: false
categories:
- CTF
- DFIR
- Memory Analysis
tags:
- Forensics
- Hashcat
- HuntressCTF
- Memory
- Volatility2
- Volatility3
---
## Bad Memory

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-24-at-3.10.27e280afpm.png?w=1024)

I spent a bit of time on this trying to get Volatility 2 to work with the Mimikatz plug-in. I was not successful. I was able to run the Volatility hashdump module.

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-24-at-3.26.49e280afpm.png?w=944)

I switched to Volatility3 and ran hashdump. For whatever reason the output of Volatility3 was different.

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-24-at-3.10.56e280afpm.png?w=1024)

The only user besides the default accounts is for 'Congo.' Copy the hashed password and head over to <https://hashes.com/en/decrypt/hash> where we can search for the hash.

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-24-at-3.11.57e280afpm.png?w=1024)

Yay, we got a match.

[Note: anecdotally I was advised that you could do this offline as well with **Hashcat** and the _rockyou_ wordlist. I had tried that earlier but was using the Volatility2 output. :( ]

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-24-at-4.17.39e280afpm.png?w=1024)

The last step is to convert 'goldfish#' to MD5. 

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-24-at-3.13.54e280afpm.png?w=1024)

Now just wrap it in the flag { } and you're good to go.

* * *

Use the tag [**#HuntressCTF**](https://bakerstreetforensics.com/tag/HuntressCTF/) on BakerStreetForensics.com to see all related posts and solutions for the 2023 Huntress CTF.
