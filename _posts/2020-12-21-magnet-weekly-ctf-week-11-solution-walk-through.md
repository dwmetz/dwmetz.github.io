---
layout: post
permalink: http://bakerstreetforensics.com/2020/12/21/magnet-weekly-ctf-week-11-solution-walk-through/
title: Magnet Weekly CTF, Week 11 Solution Walk Through
description: None
date: 2020-12-21 16:00:00 -0000
last_modified_at: 2020-12-17 21:25:24 -0000
publish: true
pin: false
categories:
- CTF
- DFIR
tags:
- CTF; Magnet; DFIR; Memory; Volatility; REMnux
- Wireshark; PCAP
---
**_Challenge 11, Part 1: What is the IPv4 address that myaccount.google.com resolves to?_**

I was able to find this pretty quick going back to last week's artifacts. In week 10, I [used bulk_extractor to carve a PCAP out of the memory image](https://bakerstreetforensics.com/2020/12/14/magnet-weekly-ctf-week-10-solution-walk-through/).

![](https://bakerstreetforensics.com/wp-content/uploads/2020/12/screen-shot-2020-12-17-at-4.12.12-pm.png?w=748)

Opening the same PCAP file I applied a String filter for 'myaccount'.

![](https://bakerstreetforensics.com/wp-content/uploads/2020/12/week-11-.png?w=1024)Wireshark viewing PCAP carved from Memory

In the highlighted row we can see a DNS resolution for myaccount.google.com coming back as **172.217.10.238**. [Flag 1]

**_Challenge 11, Part 2: What is the canonical name (cname) associated with Part 1?_**

Scrolling further to the right on the same entry, we see that the CNAME for myacccount.google.com was **www3.l.google.com**. [Flag 2]
