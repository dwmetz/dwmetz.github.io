---
layout: post
permalink: http://bakerstreetforensics.com/2023/11/02/huntress-ctf-week-2-warmups/
title: 'Huntress CTF: Week 2 - WarmUps'
description: None
date: 2023-11-02 11:00:00 -0000
last_modified_at: 2023-10-17 19:21:13 -0000
publish: true
pin: false
categories:
- CTF
- DFIR
tags:
- cookies
- CyberChef
- decode.fr
- F12
- HuntressCTF
---
## Chicken Wings

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-09-at-9.44.32e280afam.png?w=1024)

Opening the file with a text editor yields... (if you're old like me you may recognize it)

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-09-at-3.57.48e280afpm.png?w=683)

Wingdings! Head over to [dcode.fr](http://dcode.fr) and translate it.

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-09-at-9.52.17e280afam.png?w=1024)

* * *

## F12

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-10-at-10.56.05e280afam.png?w=1024)

Hit the Start button and we're provided with a URL and port.

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-10-at-9.42.48e280afam.png?w=1024)

Open the site in a browser and enable source debugging, usually "F12" as the challenge suggests.

If you click on the blue Capture The Flag button, you may observe a VERY quick pop-up.

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-10-at-9.46.07e280afam.png?w=1024)

If we scroll to the bottom of the source code, (in CTF's and Malware I always tend to hunt bottom up first), we see that the pop-up being invoked is at **./capture_the_flag.htm/**. If we append that to our current URL...

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-10-at-9.48.05e280afam.png?w=1024)

We get to our flag page. Here I right clicked on the "Your flag is:" to select **View Page Source.**

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-10-at-9.49.02e280afam.png?w=1024)

* * *

## Magic Cookies

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-11-at-9.10.53e280afam.png?w=1024)

As with previous interactive challenges, we'll start with the obvious "Start"

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-11-at-10.41.05e280afam.png?w=1024)

We have a URL and port. Let's open this in Chrome.

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-11-at-12.24.16e280afpm.png?w=1024)

Pressing cook next to one of the recipes starts a countdown timer.

F12 in Chrome will toggle the Developer options.

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-11-at-12.16.42e280afpm.png?w=1024)

Navigating to Application > Storage reveals the cookies. We have a cookie for in_oven with a Base64 value.

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-11-at-12.10.02e280afpm.png?w=1024)

This gets decoded as {"recipe": "Magic Cookies", "time": "10/11/2023, 15:50:04"}

Having also reviewed the source code it looks like this value that's representing the start of the 'baking.' Either we can wait around for 120 hours to see what happens next, or we can travel through time. Sort of.

So we know the formula for the cookie values. We can use that to generate our own cookie. Using the same text, only changing the date to 10/06/2023, we'll encode that string in Base64.

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-11-at-12.06.51e280afpm.png?w=1024)

There's a plugin for Chrome called "🍪 EditThisCookie ".

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-11-at-12.12.13e280afpm.png?w=1024)

Substitute the Base64 we generated and apply the cookie.

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-11-at-12.39.18e280afpm.png?w=1024)

Refresh the window and the flag should appear.

* * *

Use the tag [**#HuntressCTF**](https://bakerstreetforensics.com/tag/HuntressCTF/) on BakerStreetForensics.com to see all related posts and solutions for the 2023 Huntress CTF.
