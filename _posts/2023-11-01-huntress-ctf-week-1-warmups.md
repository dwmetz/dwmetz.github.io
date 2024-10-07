---
layout: post
permalink: http://bakerstreetforensics.com/2023/11/01/huntress-ctf-week-1-warmups/
title: 'Huntress CTF: Week 1 - WarmUps'
description: None
date: 2023-11-01 11:00:00 -0000
last_modified_at: 2023-10-31 01:08:23 -0000
publish: true
pin: false
categories:
- CTF
- DFIR
- PowerShell
tags:
- Forensics
- HuntressCTF
- Malware
---
![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-05-at-1.06.05e280afpm.png?w=1000)

The team at Huntress pulled off an amazing CTF that ran through the month of October with new challenges released daily. In this series, I'll be providing my solutions to the challenges. **WARNING Will Robinson, spoilers ahead!** Use the tag **[#HuntressCTF](https://bakerstreetforensics.com/tag/HuntressCTF/)** to see all related posts.

## Technical Support

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-04-at-3.04.56e280afpm.png?w=1024)

There wasn't really a solve to this one, but I'm including here for consistency. If you head to the Discord server for the event and went to the support channel, the flag was provided.

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-06-at-8.31.54e280afam.png?w=707)

* * *

## String Cheese

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-04-at-3.06.43e280afpm.png?w=1024) ![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/cheese.jpg?w=612)

Taking this literally - we'll run STRINGS on cheese:

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-04-at-3.09.31e280afpm.png?w=517)

If we scroll down through the output... 

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-04-at-3.09.00e280afpm.png?w=793)

* * *

## Notepad

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-04-at-3.10.08e280afpm.png?w=829)

Right click on the notepad file, open with VS Code or text editor of choice.

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-04-at-3.11.16e280afpm.png?w=755)

* * *

## CaesarMirror

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-05-at-9.50.44e280afam.png?w=1024)

When you examined the text file you got

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-06-at-8.41.43e280afam.png?w=719)

I copied the text over to CyberChef and started running some recipes on it. I found an algorithm that would work on it, well, one half at a time. 

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-05-at-9.51.33e280afam.png?w=1024)

I took the original file and edited it into 2 versions, caesar_left.text and caesar_right.txt. I converted each side of the file, screenshotted the output, and then aligned them next to each other to read the complete output.

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-05-at-9.47.45e280afam-1.png?w=1024)

* * *

## Book By Its Cover

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-04-at-3.13.07e280afpm.png?w=764)

Use the FILE command to get the properties of book.rar.

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-04-at-3.12.46e280afpm.png?w=767)

Hmm. A png file. Let's open that with an image viewer.

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-04-at-10.12.07e280afam.png?w=790)

* * *

## BaseFFFF+1

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-04-at-3.13.35e280afpm.png?w=652)

Examining the file contents yielded...

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-06-at-8.58.08e280afam.png?w=688)

Back to CyberChef. There's Base64 and Base85 but neither of those work. Looking closer at the title…. BaseFFFF+1… FFFF is the Hexadecimal for 65535. Add one and you have 65536. I googled Base65536, and while it’s not in CyberChef it does exist. 

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-04-at-2.43.35e280afpm.png?w=1024)

* * *

## Read the Rules

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-05-at-10.47.58e280afam.png?w=767)

Head over to the Rules page. While you’re there, be sure to read up on what tools are not allowed. CTFs are usually not the situation where you bring a tank to a knife fight. Once you’ve read everything, visible, three or four times if you’re me, right, click on the webpage and choose view source.

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/img_8868-1.png?w=1024) ![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/img_8867-1.jpg?w=678)

* * *

## Query Code

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-05-at-10.44.30e280afam.png?w=790)

Once again the FILE command gives us our first clue. 

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-05-at-10.44.04e280afam.png?w=1024)

It’s a png image so open with an image viewer and you have a QR code. Scan that with a QR reader and…

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/img_8858.jpg?w=932)

* * *

## Dialtone

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-06-at-9.05.43e280afam.png?w=745)

The provided wav file is a recording of different telephone buttons being pushed. The first thing to do is identify what buttons/numbers are being pushed. Using the site [DialABC](http://dialabc.com/sound/detect/) I uploaded the wav file and then transcribed the DTMF Tone outputs. 

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-06-at-12.02.23e280afpm.png?w=1024)

13040004482820197714705083053746380382743933853520408575731743622366387462228661894777288573\. That **_is_** on heck of a phone number!

A hint on Discord led me to the next step. It referenced that this was a BigInteger value. After several trips with Alice down various rabbit holes I found a PowerShell syntax to convert BigInt to strings.

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-06-at-11.49.34e280afam.png?w=1024)

Hmm. Looks closer to what an encoded flag might look like, but still not there yet. Back over to CyberChef and sprinkle a little Magic dust... and we see that the next and last decoding step is to From_Hex.

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-06-at-11.48.04e280afam.png?w=1024)

* * *

## Layered Security

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-07-at-9.03.00e280afam.png?w=632) ![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-09-at-8.02.36e280afam.png?w=1024)

The file command indicates that it's a GIMP image file. I recall that GIMP is an open-source application that's comparable to Adobe Photoshop. I'd used it previously but not in a long time. I also can't help but think of Pulp Fiction and "Bring out the Gimp."

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/image.jpg?w=1024)

After a morbid chuckle and a quick installation, I launch GIMP and open the file. In the bottom right we see there are a number of faces that are part of this picture.

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-09-at-8.12.19e280afam.png?w=1024)

As we peel down the layers we find the flag in one of the images.

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-07-at-1.07.13e280afpm.png?w=1024)

* * *

## Comprezz

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-08-at-9.14.17e280afam.png?w=938)

We've been pretty successful starting with the file command, so let's start there.

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-09-at-8.21.17e280afam.png?w=1024)

As the challenge suggests, no I have not heard of this file type. A quick google for _compress'd data 16 bits_ takes me to several posts on how to uncompress theses files. After a brief trial and error (it may have taken me 2 times), I cat'd the file and then piped it to uncompress.

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-09-at-8.27.51e280afam.png?w=1024)

* * *

That's it for the challenges in the **Warm Up** category. There were also challenges in **_Forensics_** , **_Malware_** and **_Miscellaneous_**. 

Use the tag **[#HuntressCTF](https://bakerstreetforensics.com/tag/HuntressCTF/)** to see all related posts. Now that October is over, I'll be releasing as many of these as I can.
