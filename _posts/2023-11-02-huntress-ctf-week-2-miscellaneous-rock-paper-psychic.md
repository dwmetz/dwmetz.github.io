---
layout: post
permalink: http://bakerstreetforensics.com/2023/11/02/huntress-ctf-week-2-miscellaneous-rock-paper-psychic/
title: 'Huntress CTF: Week 2 - Miscellaneous: Rock, Paper, Psychic'
description: None
date: 2023-11-02 21:00:00 -0000
last_modified_at: 2023-10-26 19:50:07 -0000
publish: true
pin: false
categories:
- CTF
- DFIR
- Miscellaneous
tags:
- HuntressCTF
- x64dbg
---
## Rock, Paper, Psychic

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-16-at-7.14.16e280afam.png?w=1024)

**Do you want to play a game?**

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-26-at-3.26.33e280afpm.png?w=1023)

You can see the basic flow of the game above. You put in your choice, then after some calculation the game chooses, and what do you know - the game always makes the winning choice.

**How about a nice game of Chess?**

Having played the game a couple times to get familiar with the flow, I ran the program using x64dbg. 

Hit **F9** a few times until it the program gets to your input choice.

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-26-at-3.32.39e280afpm.png?w=1024)

Once you've typed in your selection in the command window, back to x64dbg. From here we will step over (**F8**) the instructions 1 by one.

Continue to hit **F8** , observing as the rest of the game text appears. 

**Global Thermonuclear War**

In x64dbg, we see that the program tests 2 values and then does a JNE (Jump if not Equal) command to another function 416C6A.

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-26-at-2.53.16e280afpm.png?w=1024)

If we use the debugger and change this to JE (Jump if equal to):

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-26-at-2.53.54e280afpm.png?w=1024)

Who you calling cheater?

* * *

Use the tag [**#HuntressCTF**](https://bakerstreetforensics.com/tag/HuntressCTF/) on BakerStreetForensics.com to see all related posts and solutions for the 2023 Huntress CTF.
