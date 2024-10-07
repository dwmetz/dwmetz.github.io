---
layout: post
permalink: http://bakerstreetforensics.com/2023/11/03/huntress-ctf-week-3-forensics-rogue-inbox/
title: 'Huntress CTF: Week 3 - Forensics: Rogue Inbox, Texas Chainsaw Massacre: Tokyo
  Drift'
description: None
date: 2023-11-03 15:00:00 -0000
last_modified_at: 2023-10-27 20:24:23 -0000
publish: true
pin: false
categories:
- CTF
- DFIR
tags:
- Chainsaw
- CSV
- CyberChef
- Event Logs
- Forensics
- HuntressCTF
- PowerDecode
- PowerShell
---
## Rogue Inbox

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-16-at-12.10.40e280afpm.png?w=1024)

Originally I was looking at this in Timeline Explorer, but decided to switch to Excel. 

Swimming and scanning through a sea of log entries, an anomaly showed itself.

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-16-at-3.09.42e280afpm.png?w=1024)

For this one I just copied the values out by hand.

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-17-at-2.18.56e280afpm.png?w=739)

* * *

## Huntress CTF: Week 3 - Forensics: Rogue Inbox, Texas Chainsaw Massacre: Tokyo Drift

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-21-at-1.05.12e280afpm.png?w=1024)

The download is Application Logs.evtx

If you open the log with Event Viewer, you may see there's an entry for a (non-actual) event ID of 1337.

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-27-at-8.27.33e280afam.png?w=1024)

The error content isn't very helpful.

Let's take a hint from the title and run the event log through **Chainsaw**.

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-27-at-8.52.20e280afam.png?w=1024)

Nothing significant when using the stock rules. What if we poke specifically at Event ID 1337.

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-27-at-8.53.11e280afam.png?w=1024)

That looks interesting.

Copy the binary data and bring it over to CyberChef

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-27-at-8.54.04e280afam.png?w=1024)

From unintelligible binary to unintelligible PowerShell.

Copy the output and save it is a .ps1 file. We can run the script through PowerDecode.

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-27-at-2.50.01e280afpm.png?w=1024)

PowerCode works down through the obfuscation layers, finally revealing the plain text of the command.

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-27-at-2.51.32e280afpm.png?w=1024)

Now that the code has been deobfuscated, time to figure out what it does. I copied the code into PowerShell ISE and start isolating the different command sections.

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-27-at-2.54.06e280afpm.png?w=1024)

One of the commands does a DNS lookup and directs the output into a string.

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-27-at-4.16.58e280afpm.png?w=1024)

If we run the command on its own we can see the output. The last part of the script checks to see if the output matches the pattern of a Base64 encoded string, and if so, decodes it.

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-27-at-2.55.09e280afpm.png?w=1024)

Now what was that about Tokyo?

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/tcm.jpg?w=1024)

* * *

Use the tag [**#HuntressCTF**](https://bakerstreetforensics.com/tag/HuntressCTF/) on BakerStreetForensics.com to see all related posts and solutions for the 2023 Huntress CTF.
