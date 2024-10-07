---
layout: post
permalink: http://bakerstreetforensics.com/2024/03/12/magnet-virtual-summit-2024-capture-the-flag/
title: MAGNET Virtual Summit 2024 Capture the Flag
description: None
date: 2024-03-12 16:11:12 -0000
last_modified_at: 2024-03-12 17:38:20 -0000
publish: true
pin: false
categories:
- CTF
- DFIR
tags:
- CyberChef
- dcode.fr
- Forensics
- iLEAPP
- Magnet
- MVS2024
---
I've been participating in the MAGNET sponsored Capture the Flag (CTF) events since before being happily employed there. In a way you could say that one helped facilitate the other, but that's a story for another time. This blog actually started back in 2020 to, among other things, share my write-ups of that years CTF. 

![](https://bakerstreetforensics.com/wp-content/uploads/2024/03/mvs24_blogheader_ctf-1.png?w=1024)

The 2024 CTF event was part of the Virtual Summit that ran from February 27th to March 7th. There were more than 50 presentations about topics like mobile forensics, artificial intelligence, eDiscovery, malware, ransomware, digital evidence review, video forensics, and live Q&A sessions. 

If you missed my talk on **[Investigating Malware With Free Tools and Magnet AXIOM Cyber](https://www.magnetforensics.com/resources/investigating-malware-with-free-tools-and-magnet-axiom-cyber/)** , you can now watch that and all the other recordings at the [2024 Replays](https://www.magnetforensics.com/magnet-virtual-summit-2024-replays/) site.

The CTF questions were divided into three groups, iOS, Android & Ciphers. The evidence sources included a full file system extraction of an iPhone 14, a logical extraction of an Android phone, a Facebook 'Download Your Data' export and an export of Discord messages. I focused almost entirely on the iOS questions, and even had a few of those left on the table when the 3 hours allotted for the challenge was up. The numbers in parenthesis represent the point value which is intended to align to question difficulty. I processed the iOS extraction with AXIOM Cyber and iLEAPP.

![](https://bakerstreetforensics.com/wp-content/uploads/2024/03/screenshot-2024-03-12-at-8.22.43e280afam.png?w=1024)

## MVS 2024 CTF: iOS

**Why are your messages green?** (5)

![](https://bakerstreetforensics.com/wp-content/uploads/2024/03/green_q.png?w=516)

For this one we'll use MAGNET Axiom, specifically the Conversation View. In the message thread below, we can determine from the conversation that the first time the two persons met was December 17, 2003.

![](https://bakerstreetforensics.com/wp-content/uploads/2024/03/green-evidence.png?w=1024)

* * *

**Where /r u going on Safari?** (5)

![](https://bakerstreetforensics.com/wp-content/uploads/2024/03/twitch.png?w=533)

Examining the users Safari history we see that the user visited the url https://www.reddit.com/r/Twitch

![](https://bakerstreetforensics.com/wp-content/uploads/2024/03/safari_a.png?w=1024)

* * *

**IMAGEine living in pain**(5)

![](https://bakerstreetforensics.com/wp-content/uploads/2024/03/imagine_q.png?w=510)

The question title suggests (not so surreptitiously) that we're going to be dealing with an image file. In the MEDIA > Photos Media Information we see a picture of a store shelf of a pain relief gel. _(I know the feeling. Take care of yourself young forensicators; and don't forget the sunscreen.)_ The price of the item was $10.99.

![](https://bakerstreetforensics.com/wp-content/uploads/2024/03/pain_a.png?w=1024)

* * *

**Answer the call** (5)

![](https://bakerstreetforensics.com/wp-content/uploads/2024/03/answer_q.png?w=519)

In the Refined Results for Web Chat URLs we see the user visiting a Discord server with the guild ID of 136986169563938816.

![](https://bakerstreetforensics.com/wp-content/uploads/2024/03/guild_a.png?w=1024)

* * *

**Don't ghost me** (5)

![](https://bakerstreetforensics.com/wp-content/uploads/2024/03/ghost_q.png?w=522)

To solve this one we're first going to need to know what MYAI refers to. Running a global search for MYAI shows that it's a SnapChat "Artificial Intelligence" bot. Again we'll switch to Conversation View. Once we do so we can see that Chadwick was annoyed with MYAI on December 26th at 11:27:45 UTC.

![](https://bakerstreetforensics.com/wp-content/uploads/2024/03/myai_a.png?w=1024)

* * *

**Build me up, buttercup** (5)

![](https://bakerstreetforensics.com/wp-content/uploads/2024/03/build_q.png?w=543)

For this question I found it easier to produce the result from the iLEAPP report. What I found interesting is identifying all the other locations where the build ID of the device may be captured, like in the user's YouTube playback history.

![](https://bakerstreetforensics.com/wp-content/uploads/2024/03/butter_a1.png?w=1024) ![](https://bakerstreetforensics.com/wp-content/uploads/2024/03/butter_a2.png?w=1024)

* * *

**Warning Signs** (5)

![](https://bakerstreetforensics.com/wp-content/uploads/2024/03/warning_q.png?w=520)

In order to get this flag we need to combine two iOS iMessage events. We see that the user joined Boost Mobile on November 29th. The warning about reaching maximum data usage was received on December 27. There are 18 days between those dates.

![](https://bakerstreetforensics.com/wp-content/uploads/2024/03/warn_a1.png?w=1024) ![](https://bakerstreetforensics.com/wp-content/uploads/2024/03/warn_a2a.png?w=1024)

* * *

**One is The Loneliest Number** (10)

![](https://bakerstreetforensics.com/wp-content/uploads/2024/03/one_q.png?w=511)

The answer for this one can be found in the iOS snapshots on the device. This is often an interesting artifact for me as you get a glimpse (literally) into the applications that have been used on the device. These snapshot images are recorded whenever a user switches between one application and another, and is what produces the carousel like view when switching apps. It looks like Chad's feeling a little short on friends. I can sympathize at times. Meanwhile the advice from ChatGPT is good advice for making and maintaining connections in the DFIR community as well. 

![](https://bakerstreetforensics.com/wp-content/uploads/2024/03/ai_a.png?w=1024)

* * *

**For when I can't Find My gear** (10)

![](https://bakerstreetforensics.com/wp-content/uploads/2024/03/gear_q.png?w=517)

Drilling into the Cached Locations and examining in World map view, we see a cluster of activity around the Neptune Mountaineering. (You'll also be able to find that Chad connected to their Guest Wi-Fi when he was visiting the store.)

![](https://bakerstreetforensics.com/wp-content/uploads/2024/03/find_a.png?w=1024)

* * *

**Just a couple steps away**(10)

![](https://bakerstreetforensics.com/wp-content/uploads/2024/03/steps_.png?w=512)

Apple Health Steps is one of the artifacts found under Connected Devices. If we apply a filter for just events on 12/3, we see four values recorded. Add the four together and you get the total steps for the day.

![](https://bakerstreetforensics.com/wp-content/uploads/2024/03/steps_a.png?w=1024)

* * *

**I hear Stanley cups are all the rage** (25)

![](https://bakerstreetforensics.com/wp-content/uploads/2024/03/stanley_q.png?w=516)

While perusing the photos I saw that there was one captured at a hockey game on December 22. In the image we can see that the game took place at the Ball Arena. 

![](https://bakerstreetforensics.com/wp-content/uploads/2024/03/stanley1.png?w=1024)

My sports knowledge is on par with my cooking abilities - not good. I decided to 'phone a friend' to help with this one, the Google Bard (now Gemini) AI.

![](https://bakerstreetforensics.com/wp-content/uploads/2024/03/stanley_a2.png?w=910)

* * *

**Can anyone Kelp?** (25)

![](https://bakerstreetforensics.com/wp-content/uploads/2024/03/kelp_q.png?w=517)

If you filter out the applications from apple (com.apple...) there aren't too many remaining, and of those only a few are games. Of those I can only see one dealing with _greens_.

![](https://bakerstreetforensics.com/wp-content/uploads/2024/03/screenshot-2024-03-12-at-10.44.16e280afam.png?w=1024)

The name of the application Terrarium was not accepted for an answer. Checking iLEAPP to see if there was another application that I had missed, I saw the full name of the game is Terrarium: Garden Idle. It's a good idea to always validate your evidence with at least one addition tool from your primary.

![](https://bakerstreetforensics.com/wp-content/uploads/2024/03/can_anyone_kelp-a.png?w=1024)

* * *

**The easy way or the hard way** (25)

![](https://bakerstreetforensics.com/wp-content/uploads/2024/03/hard_q.png?w=521)

Again looking at the chat history we have a conversation between Chad an Rocco. The last message sent was on December 21, 2023 at 06:29:36 UTC.

![](https://bakerstreetforensics.com/wp-content/uploads/2024/03/hard_a.png?w=1024)

* * *

**Follow the Breadcrumbs**(50)

![](https://bakerstreetforensics.com/wp-content/uploads/2024/03/bread_q.png?w=522)

This answer was easier to grab from iLEAPP as there's a specific entry for Biome Text Input Sessions. Filtering for amazon, we see 4 entries. 2 of those occurred on December 24.

![](https://bakerstreetforensics.com/wp-content/uploads/2024/03/amazon.png?w=1024)

* * *

**Season's Greetings** (75)

![](https://bakerstreetforensics.com/wp-content/uploads/2024/03/season.png?w=524)

Start off with a search for Susan and we can see there's a iMessage chat history. Chadwick's first message to Susan says "Christmas Susan! 🪴 how have you been?"

![](https://bakerstreetforensics.com/wp-content/uploads/2024/03/seasons_a.png?w=1024) ![](https://bakerstreetforensics.com/wp-content/uploads/2024/03/season_a2.png?w=1000)

* * *

## MVS 2024 CTF: Ciphers

While working through the iOS questions I diverted my attention to a few of the Cipher questions when I needed to give my brain a change of pace. I only did a few of them.

**Have you ever tried reading the alphabet in reverse?** (5)

![](https://bakerstreetforensics.com/wp-content/uploads/2024/03/reverse_q.png?w=540)

For this one we'll throw the sample text into dcode.fr. Doing so suggests it is an Atbash Cipher.

"Atbash (Mirror code), a substitution cipher replacing the first letter of the alphabet with the last, the second with the penultimate etc."

That sounds to me like a backwards alphabet. Decode the text using the Atbash Cipher on dcode.fr.

![](https://bakerstreetforensics.com/wp-content/uploads/2024/03/screenshot-2024-03-12-at-11.38.58e280afam.png?w=1009)

* * *

**Why did the bicycle fall over? It was tired of all the ROTation!**(5)

![](https://bakerstreetforensics.com/wp-content/uploads/2024/03/rot_q.png?w=519)

From the clue we can be pretty sure this is a ROT cipher. Using CyberChef we can try the ROT13 Brute Force. Scanning through the output we see that the output for a rotation of 2 produces a legible result and is the answer for the challenge.

![](https://bakerstreetforensics.com/wp-content/uploads/2024/03/rot_a.png?w=1024)

* * *

**VIGorous ENcrypting? Embrace the Riddle's Essence, it's "essential"!** (10)

![](https://bakerstreetforensics.com/wp-content/uploads/2024/03/vig_q.png?w=522)

A quick Googling on VIG and cipher and we learn there's a Vigenère cipher.

![](https://bakerstreetforensics.com/wp-content/uploads/2024/03/vig1.png?w=690)

Off to CyberChef. Choose the Vigenère cipher recipe, enter the input provided in the question, QshprMzepw, and use the key "essential". The decoded text is MapleTrees.

![](https://bakerstreetforensics.com/wp-content/uploads/2024/03/vig_a.png?w=1024)

* * *

That's all for me. Thanks to Jessica Hyde and her team at Hexordia and the students at Champlain College that put all the effort into coming up with the challenges. Also thanks to the winningest Kevin who took the year off from competition to join the CTF creation team.

As always it was a lot of fun, and I learned a couple things along the way.
