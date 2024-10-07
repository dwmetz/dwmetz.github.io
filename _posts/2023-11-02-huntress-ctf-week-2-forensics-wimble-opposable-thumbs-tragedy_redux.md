---
layout: post
permalink: http://bakerstreetforensics.com/2023/11/02/huntress-ctf-week-2-forensics-wimble-opposable-thumbs-tragedy_redux/
title: 'Huntress CTF: Week 2 - Forensics: Wimble, Opposable Thumbs, Tragedy_Redux'
description: None
date: 2023-11-02 19:00:00 -0000
last_modified_at: 2023-10-23 20:23:20 -0000
publish: true
pin: false
categories:
- CTF
- DFIR
tags:
- Forensics
- HuntressCTF
- Magnet
- olevba
- prefetch
- thumbcache
- vbs
---
## Wimble

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-10-at-9.02.04e280afam.png?w=814)

Once the file was downloaded and extracted from the zip I ran the file command on it.

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-10-at-10.38.46e280afam.png?w=899) ![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-10-at-9.06.06e280afam.png?w=684)

OK so we'll be doing the analysis for this one on a Windows box to start.   
  
Move the file to windows and rename to Fetch.wim

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-10-at-9.08.33e280afam.png?w=76)

Open the .wim with 7zip explorer

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-10-at-10.03.45e280afam.png?w=1024)

Within the zip file we see a plethora of Prefetch (.pf) files, but among them we there is a fetch.zip

When we extract the contents of the zip file we have another directory of Prefetch files.

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-10-at-10.24.49e280afam.png?w=1024)

I extracted the .pf files to a folder. 

I used Magnet AXIOM to process the prefetch files. Based on our scenario, I have keywords set for _Huntress_ , _ctf_ , and _flag_.

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-10-at-10.16.57e280afam.png?w=1024)

_That was easy._

* * *

## Opposable Thumbs

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-16-at-7.07.44e280afam.png?w=1024)

I know for a fact that Axiom can process thumbnail caches.

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-16-at-7.08.47e280afam.png?w=1024)

And BAM! there's the flag.

* * *

## Tragedy Redux

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/question.png?w=965)

First things first, let's get an idea of what kind of file we're dealing with. Hmm. It shows as a zip archive. When the file is unzipped we see the structure below.

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/steps1-2.png?w=1024)

Looking at the structure, as seasoned analyst may identify that the tragedy_redux file is in fact a word document. Which will bring up another method in a minute. But before that let's take a look at the vbaProject.bin file with olevba.

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/steps3olevba.png?w=1024)

There's a macro file with some curious fruit and vegetable related functions. 

If you realized at the beginning this was a word doc file, you could append the file extension .docm to the file.

When opening the file in Word, there is a prompt to enable macros.

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/macro_warning.png?w=444)

Once the document is open you see a document containing the definition of Tragedy.

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-23-at-4.04.59e280afpm.png?w=670)

From there we can go to Tools > Macros > Edit... we can get to the same vbs content we did with olevba.

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/screenshot-2023-10-23-at-4.07.01e280afpm.png?w=1024)

The next step was to convert the vbs into something actionable. I struggled on this one, but one of my teammates was successful in converting the vbs to Python.

This code interprets the numeric values in `longstring` (Apples), as decimal representations of ASCII values, subtracts 17 from each value, and prints the corresponding characters. The characters are printed one by one without newlines, forming a string of characters as the output.

![](https://bakerstreetforensics.com/wp-content/uploads/2023/10/steps4-.png?w=1024)

When we run the Python script we get back:
    
    
    powershell -enc JGZsYWc9ImZsYWd7NjNkY2M4MmMzMDE5Nzc2OGY0ZDQ1OGRhMTJmNjE4YmN9Ig==

Now we can echo the value to base64 decrypt and get our final flag value.

* * *

Use the tag [**#HuntressCTF**](https://bakerstreetforensics.com/tag/HuntressCTF/) on BakerStreetForensics.com to see all related posts and solutions for the 2023 Huntress CTF.
