---
layout: post
permalink: http://bakerstreetforensics.com/2020/10/19/magnet-ctf-question-2-solution-walk-through/
title: 'Magnet CTF: Question 2 Solution Walk-Through'
description: None
date: 2020-10-19 15:04:09 -0000
last_modified_at: 2020-11-30 02:35:23 -0000
publish: true
pin: false
categories:
- CTF
- DFIR
tags:
- Magnet;DFIR;MagnetWeeklyCTF
---
**_Challenge 2 (OCT 12-18) PIP Install: What domain was most  recently viewed via an app that has picture-in-picture capability?_**

For the week 2 challenge, we’re using the same Android image we examined last week. From the question there’s two factors involved, application capability and application usage.

The first thing to understand is what applications on the device have PIP (Picture in Picture) capability.

![Picture2](http://bakerstreetforensics.com/wp-content/uploads/2020/10/picture2.png)

I reviewed the installed applications on the device looking for anything that could be PIP related. A screen recording application caught my attention (googled to understand capability), but **_this was not the ‘Droid (app)  we’re looking for._**

One of the Discord users shared this helpful link describing the capability and what applications featured PIP support.

https://nokiapoweruser.com/list-apps-support-oreo-picture-picture-mode-enable/ 

Back to the applications I tagged anything listed that showed up in the PIP article.

From here I proceeded to look at the **Application  Activity – Android.**  One of the applications we can see recent activity for is Google Chrome. From the previously referenced article we know this app supports PIP.

![Picture3](http://bakerstreetforensics.com/wp-content/uploads/2020/10/picture3.png)

In the preview card we can see that it was captured that **Chrome** was used to visit a URL beginning with **MallieSae.com.  **

The artifact for this can be found at **MUS_Android.tar\data\system_ce\0\snapshots\320.jpg**

Answer: Google Chrome, an app that supports PIP, was recently used visit the domain malliesae.com

PDF: [MagnetWeeklyCTF Write-Up 2.pdf](http://bakerstreetforensics.com/wp-content/uploads/2020/10/magnetweeklyctf-write-up-2.pdf "MagnetWeeklyCTF Write-Up 2.pdf")
