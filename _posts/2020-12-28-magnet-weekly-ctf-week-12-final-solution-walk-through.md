---
layout: post
permalink: http://bakerstreetforensics.com/2020/12/28/magnet-weekly-ctf-week-12-final-solution-walk-through/
title: Magnet Weekly CTF, Week 12 Solution Walk Through
description: None
date: 2020-12-28 16:11:00 -0000
last_modified_at: 2020-12-28 23:22:03 -0000
publish: true
pin: false
categories:
- CTF
- DFIR
tags:
- Axiom
- Forensics
- Magnet
- Memory
- MemprocFS
- REMnux
- VirusTotal
- Volatility
---
**_The final challenge (#12) - Part 1:_**

**_What is the PID of the application where you might learn "how hackers hack, and how to stop them"?_**

**_Format: #### Warning: Only 1 attempt allowed!_**

The first thing I did was open the memdump file in HxD Hex Editor. A quick search found several hits. 

![](https://bakerstreetforensics.com/wp-content/uploads/2020/12/memdump-in-hxd.png?w=1024)

I considered mapping the Offset back to the process memory but before going down that road (anticipating it to be math heavy) I decided to drop the individual process memory instead. Looking at the text surrounding "How Hackers Hack..." it appears to be html code. Looking even closer I'd say that it was in response to a search request for "**how to stop getting hacked over and over**." Based on that I knew I'd be looking for a browser process.

Running **pslist** in Volatility we see that there's multiple browser processes running for both Chrome and Internet Explorer.

![](https://bakerstreetforensics.com/wp-content/uploads/2020/12/pslist.png?w=1024)

I decided to focus on the iexplore.exe processes for Internet Explorer first - for 2 reasons. 1 - there were less running than Chrome so it was a smaller set to work through first. 2 - I did happen to find a **_Parsed Search Query_** in Axiom for "how to stop getting hacked over and over." 

![](https://bakerstreetforensics.com/wp-content/uploads/2020/12/axiom-hack.png?w=1024)

The URL indicates a search from Bing.com. Only a sociopath would use Bing to search within Chrome so Internet Explorer it is.

I used the **memdump** Volatility plugin to dump the process memory for both IE processes.

![](https://bakerstreetforensics.com/wp-content/uploads/2020/12/memdump-ie.png?w=904)

Next I ran **strings** against each dump file to see if there was a hit.

![](https://bakerstreetforensics.com/wp-content/uploads/2020/12/strings-ie-dumps.png?w=1024)

We see that in the second file 4480.dmp (associated with PID 4480) contains the content we're looking for. **_What is the PID of the application where you might learn "how hackers hack, and how to stop them"?_** **4480 [Flag 1]**

* * *

**_The final challenge (#12) - Part 2:_**

**_What is the product version of the application from Part 1?_**

**_Format: XX.XX.XXXX.XXXXX_**

OK, so we need to know what version of Internet Explorer was used for the Bing search. Off to the Google to find that the IE version information is stored in the registry in HKEY_LOCAL_MACHINE\Software\Microsoft\Internet Explorer in the **svcVersion** value.

![](https://bakerstreetforensics.com/wp-content/uploads/2020/12/windows-build-article.png?w=1024)

From here I mount the full memory image using **MemprocFS.**

![](https://bakerstreetforensics.com/wp-content/uploads/2020/12/memprocfs-ie-registry.png?w=1024)

Using the file structure to navigate to the registry key I open svcVersion.txt and verify that the IE version running is 11.0.9600.18860. Back to the scoreboard to submit the bittersweet ending to a very fun challenge and ..... WRONG.

![](https://bakerstreetforensics.com/wp-content/uploads/2020/12/tenor.gif?w=244)

Hmm, so everything I knew (which was limited to be honest) told me that I had the version right, but that wasn't the right answer. Over on the Discord channel I saw I wasn't the only one to have the same quandry.

I waited and lurked, waited and lurked - but wasn't seeing any update to the question. The following day while meditating on the matter in the shower I was thinking about what other means existed to identify details like this. 

I used the **procdump** Volatility plugin to dump the process executable for PID 4480.

![](https://bakerstreetforensics.com/wp-content/uploads/2020/12/procdump-4480.png?w=1024)

Once I had executable.4480.exe I uploaded the file to Virus Total.

![](https://bakerstreetforensics.com/wp-content/uploads/2020/12/vt-file-info.png?w=1024)

Scrolling down on the details tab we see that the exe is correctly identified as Internet Explorer and shows a File Version of 11.00.9600.18858. This is very similar to what we identified earlier (...58 vs ...60). 

Answer: **11.00.9600.18858** [Flag 2] **_CORRECT!_**

I'll be very interested to learn how others who got the flag identified the correct version information. I suspect there's additional artifacts that I didn't explore that hold those clues but for the time being - it's a mystery to me.

![](https://bakerstreetforensics.com/wp-content/uploads/2020/12/1lai6f.jpg?w=507)

Who am I kidding? It's gonna be killing me til I know the answer.
