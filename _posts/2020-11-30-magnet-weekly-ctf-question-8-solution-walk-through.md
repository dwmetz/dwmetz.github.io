---
layout: post
permalink: http://bakerstreetforensics.com/2020/11/30/magnet-weekly-ctf-question-8-solution-walk-through/
title: 'Magnet Weekly CTF: Question 8 Solution Walk Through'
description: None
date: 2020-11-30 16:01:00 -0000
last_modified_at: 2020-11-30 16:31:16 -0000
publish: true
pin: false
categories:
- CTF
- DFIR
tags:
- Magnet; DFIR; Forensics; CTF; Hadoop; Linux
---
I only managed to get half the solution to last weeks challenge.

![](https://bakerstreetforensics.com/wp-content/uploads/2020/11/screen-shot-2020-11-29-at-8.53.17-am.png?w=494)

Finding the first half of the solution was pretty straight-forward.

In the File system view (or via your image mounting directory traversal of choice) navigate to **\var\log\apt.**

![](https://bakerstreetforensics.com/wp-content/uploads/2020/11/screen-shot-2020-11-29-at-9.00.08-am.png?w=1024)

Here we find the **history.log** file which keeps track of applications installed via apt.

![](https://bakerstreetforensics.com/wp-content/uploads/2020/11/screen-shot-2020-11-29-at-9.02.34-am.png?w=574)

If you scroll to the end of the log you can see that a lot of packages were installed or upgraded on 2017-11-08. From there on we have no (logged) activity until 2019-10-07 when **php** [Flag] is installed.

![](https://bakerstreetforensics.com/wp-content/uploads/2020/11/screen-shot-2020-11-29-at-9.05.17-am.png?w=499)

Part 2 I'm afraid eluded me. I'm looking forward to seeing the other write-ups to see the solve for the **Why?**

 
