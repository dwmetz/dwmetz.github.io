---
layout: post
permalink: http://bakerstreetforensics.com/2023/11/01/huntress-ctf-week-1-forensics-backdoored-splunk-traffic-dumpster-fire/
title: 'Huntress CTF: Week 1 - Forensics: Backdoored Splunk, Traffic, Dumpster Fire'
description: None
date: 2023-11-01 13:00:00 -0000
last_modified_at: 2023-10-22 11:13:13 -0000
publish: true
pin: false
categories:
- CTF
- DFIR
- PowerShell
- Python
tags:
- Curl
- CyberChef
- DNS
- Firepwd
- Forensics
- HuntressCTF
- rita
- Splunk
- the_silver_searcher
---
## Backdoored Splunk

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-07-at-9.03.22e280afam.png?w=1024)

Hit Start. 

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-09-at-11.03.22e280afam.png?w=1024)

So we've got a url and a specific port. Firefox web browser yields...

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-09-at-11.07.22e280afam.png?w=1024)

So we need an Authorization header. 🤔

Time to look at the provided files. It looks to be the export of a Splunk application. 

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-09-at-11.13.43e280afam.png?w=976)

Time to download an eval copy of Splunk and... pause. There's probably a simpler way to attack this.

[The Silver Searcher](https://github.com/ggreer/the_silver_searcher) is a command line tool I picked up during the CTF and I love it. It's like Grep on PCP.

Once installed, the base command is ag, followed by what you're searching for, and where. So let's do a quick search for Authorization on all the contents of this directory.

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-09-at-11.19.44e280afam.png?w=1024)

That looks interesting. A clue? One of the PowerShell scripts has **_Authorization_** and what looks to be Base64 code.

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-07-at-3.36.59e280afpm.png?w=1024)

We also see a comment about the $PORT being dynamic based on the Start button. Decoding the string in CyberChef...

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-07-at-3.37.53e280afpm.png?w=1024)

At this point we have all the pieces, we just need to put them together. I started to look at different ways to pass an Authorization header to a web server. There's proxy tools galore. And then there's the basic's like curl. After a bit of brushing up on my syntax I had: 
    
    
    curl -H "Authorization: Basic [longStringFromThePowershell]" http://site:$PORT

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-07-at-3.36.00e280afpm.png?w=1024)

Yay what looks like more Base64. Once more with our Chef's hat and...

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-07-at-3.38.37e280afpm-1.png?w=1024)

* * *

## Traffic

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-05-at-10.51.42e280afam.png?w=926)

[rita](https://github.com/activecm/rita) was a tool I hadn't used before but it was very easy to use. I installed it on my REMnux box and then ran it against the dataset.

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-05-at-9.00.44e280afam.png?w=1000)

I then used the command to generate an html report.

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-05-at-10.51.07e280afam.png?w=696) ![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-09-at-12.00.07e280afpm.png?w=1024)

Looking through the DNS requests there's something sketchy indeed.

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-05-at-10.50.28e280afam.png?w=703)

Let's go take a look at that.

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-05-at-10.52.54e280afam.png?w=1024)

* * *

## Dumpster Fire

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-09-at-12.08.35e280afpm.png?w=1024)

Let's start with the_silver_searcher again and see if we have any luck with "Password".

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-08-at-12.27.45e280afpm.png?w=581) ![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-08-at-12.26.51e280afpm.png?w=1024)

There's a number of hits including references to an _**encryptedUsername**_ and _**encryptedPassword**_ in the logins.json file. So we've got some encrypted Firefox user passwords. If only there were a utility that could decrypt those. Enter [firepwd.py](https://github.com/lclevy/firepwd#firepwdpy), an open source tool to decrypt Mozilla protected passwords.

Run the script in Python and point it to the directory for the user profile (where the logins.json file is).

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-09-at-12.25.09e280afpm.png?w=1024) ![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-08-at-12.48.26e280afpm.png?w=1024)

That's a pretty LEET password ;)

* * *

Use the tag [**#HuntressCTF**](https://bakerstreetforensics.com/tag/HuntressCTF/) on BakerStreetForensics.com to see all related posts and solutions for the 2023 Huntress CTF.
