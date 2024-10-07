---
layout: post
permalink: http://bakerstreetforensics.com/2020/12/07/magnet-weekly-ctf-question-8-solution-walk-through-2/
title: 'Magnet Weekly CTF: Question 9 Solution Walk Through'
description: None
date: 2020-12-07 16:01:00 -0000
last_modified_at: 2020-12-09 20:52:50 -0000
publish: true
pin: false
categories:
- CTF
- DFIR
tags:
- CTF; Magnet; DFIR; Memory; Volatility; REMnux
---
This weeks challenge was the first of the challenges to deal with Memory forensics. The 'question' provided by Aaron Sparling (@OSINTlabworks) was a 7 parter! 

For most of the challenges so far I've been using Magnet Axiom and supplementing with other tools as needed. For this weeks solution you'll see that all is being done via Volatility using the REMnux platform. 

**_Part 1: The user had a conversation with themselves about changing their password. What was the password they were contemplating changing too. Provide the answer as a text string._**

![](https://bakerstreetforensics.com/wp-content/uploads/2020/12/screen-shot-2020-12-07-at-7.27.53-am.png?w=1024)

Originally the clue of "conversation" had me looking at Slack as it's a communications tool. With no luck there I began to investigate WINWORD.exe (MS Word). First off, we dump the process memory:

![](https://bakerstreetforensics.com/wp-content/uploads/2020/12/3180-proc-dump.png?w=1024)

Next we can use strings against the process dump and use grep to filter for 'password.'

![](https://bakerstreetforensics.com/wp-content/uploads/2020/12/grep-proc-dump-for-pw.png?w=1024)

Apparently the 'coversation' the user was having with themselves was in a Word document. Within the document the user says: "Hmmm mmaybe I should change my password to: **wow_this_is_an_uncrackable_password.** " [Flag 1]

**_Part 2: What is the md5 hash of the file which you recovered the password from?_**

The **dumpfiles** Volatility plugin can be used to dump out any files that were opened in relation to the particular memory process. There are 2 tmp (temp) files associated with Word. We can **md5sum** the exported files to get their hashes. _Bonus if you spot where it shows I spend more time in Powershell._

![](https://bakerstreetforensics.com/wp-content/uploads/2020/12/md5-tmp.png?w=1024)

The `WRD0000.tmp.dat has the MD5 has of **af1c3038dca8c7387e47226b88ea6e23**. [Flag 2]

**_Part 3: What is the birth object ID for the file which contained the password?_**

We use the **mftparser** Volatility plugin to dump the MFT (Master File Table) to a text file. 

![](https://bakerstreetforensics.com/wp-content/uploads/2020/12/birth-obj-id.png?w=1024)

Here we can see that the temp file which is holding the Autorecovery information for the open word document has the Birth Object ID: **31013058-7f31-01c8-6b08-210191061101**. [Flag 3]

**_Part 4: What is the name of the user and their unique identifier which you can attribute the creation of the file document to? Format: #### (Name)_**

![](https://bakerstreetforensics.com/wp-content/uploads/2020/12/user-id.png?w=1024)

Using the **getsids** plugin againts the PID for Word (3180), we see the the WINWORD.EXE process was executed under the context of **1000 (Warren)**. [Flag 4]

**_Part 5: What is the version of software used to create the file containing the password? Format ## (Whole version number, don't worry about decimals)_**

Using the **filescan** Volatility plugin to locate "Word" we see that WINWORD.EXE is being called from \Program Files\Microsoft Office\Office**15**. [Flag 5]

![](https://bakerstreetforensics.com/wp-content/uploads/2020/12/filescan-word.png?w=1024)

Some basic google searching informed me this directory corresponds with the installation of Office 2013, which includes Word **2013**. 

**_Part 6: What is the virtual memory address offset where the password string is located in the memory image? Format: 0x########_**

This one had me struggling for bit. During the week I heard that Aaron was presenting a webcast on Friday, **[When your forensic tool only tells part of the story; finding code injection using memory analysis Webcast](https://www.sans.org/webcasts/forensic-tool-tells-story-finding-code-injection-memory-analysis-117125)** , and that if you watched closely there may be helpful workflow.

Sure enough there was a segment where Aaron is utilizing yara rules to find the virtual offset for a string in memory, which I applied to our identified password string.

![](https://bakerstreetforensics.com/wp-content/uploads/2020/12/flag-6.png?w=1024)

In this case the offset we are looking for is **0x2180a2d**. [Flag 6]

**_Part 7: What is the physical memory address offset where the password string is located in the memory image? Format: 0x#######_**

Unfortunately, I wasn't able to get the flag for the last part of the challenge before time ran out. For certain I'll be reviewing the other walk-throughs published this week. I'm looking forward the upcoming memory challenges as well. 

https://twitter.com/dwmetz/status/1335660828805259269?s=21 
