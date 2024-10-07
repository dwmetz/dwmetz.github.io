---
layout: post
permalink: http://bakerstreetforensics.com/2020/11/09/magnet-weekly-ctf-question-5-solution-walk-through/
title: 'Magnet CTF: Question 5 Solution Walk-Through'
description: None
date: 2020-11-09 21:15:47 -0000
last_modified_at: 2020-11-30 02:31:55 -0000
publish: true
pin: false
categories:
- CTF
- DFIR
tags:
- Magnet; DFIR; Forensics; CTF; Hadoop; Linux
---
![](https://bakerstreetforensics.com/wp-content/uploads/2020/11/hadoop.png?w=540)

So for week 5 we started Ali Hadi's Linux image, (farewell for now Android.) I've worked WITH Linux for years as my underlying operating system for forensics, but as the forensics target, not so much. 

As the Magnet Training team is fond to say, "You don't know what you don't know." That was certainly the feeling for me when I opened up the week 5 challenge.

**_Challenge 5 (Nov. 2-9) Had-A-Loop Around the Block: What is the original filename for block 1073741825?_**

I knew this had something to do with data architecture but not much else. I scoured the filesystem and find some references to block_1073741825, but nothing related to file associations.

![](https://bakerstreetforensics.com/wp-content/uploads/2020/11/1-check-logs.png?w=1024)

Midway through the week, a hint was dropped. It cost 20 points but I knew that without a pointer in the right direction this was going to elude me. 

![](https://bakerstreetforensics.com/wp-content/uploads/2020/11/unlock-hint.png?w=490)

The hint wound up being a link to a presentation from the DFIR Summit in 2016.

https://www.youtube.com/watch?v=Mc0uiKmYIpk 

I watched this several (countless?) times. I think by the end after infinite repetitions my wife and cats were starting to grasp Hadoop. _I'll be looking out for other talks by Kevvie in the future._

There were 2 main takeaways for me that wound up getting me to the correct solution before the final bell tolled.

![](https://bakerstreetforensics.com/wp-content/uploads/2020/11/video-1.png?w=1024)

  1. **hdfs-site.xml** \- this file will tell you where within the system the data resides.



So I parsed the .E01's in Axiom (as Windows images) and found the hdfs-site.xml in the File system view.

![](https://bakerstreetforensics.com/wp-content/uploads/2020/11/hdfs-site-ax.png?w=1024)

I exported the file out and opened with VS Code. Lately I've been finding this just as useful if not more as Notepadd++ when dealing with text or xml files.

![](https://bakerstreetforensics.com/wp-content/uploads/2020/11/hdfs-site-vs.png?w=1024)

We're looking for the namenode path, seen here as **/usr/local/hadoop/hadoop2_data/hdfs/namenode...**

This brings me to take-away #2 from the video. 

![](https://bakerstreetforensics.com/wp-content/uploads/2020/11/video-2.png?w=1024)

The transaction logs, which capture the to and fro of files on the system, can be exported from it's native format to a human-readable xml - when done with a utility on the Hadoop server. 

![](https://bakerstreetforensics.com/wp-content/uploads/2020/11/edit-logs.png?w=1024)

Using the identified path information I exported the transaction logs via Axiom.

![](https://bakerstreetforensics.com/wp-content/uploads/2020/11/edit-logs-exported.png?w=1024)

The video calls out the usage of OEV (Offline Edits Viewer). 

[OEV documentation](https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/HdfsEditsViewer.html)

My next step was to get a Hadoop VM so that I could utilize the OEV tool.

![](https://bakerstreetforensics.com/wp-content/uploads/2020/11/bitnami-vm.png?w=795)

After a pretty basic setup I transferred the exported edit logs via SCP to the VM. Once I had the transaction log files on the VM I used the OEV utility to convert to xml.

**hdfs oev -I [edit_log_name] -o [export_name].xml -p XML**

![](https://bakerstreetforensics.com/wp-content/uploads/2020/11/oev-conversion.png?w=1024)

I then SCP'd the xml files back to my analysis machine and ran a search for the block number in VS Code on the file directory.

![](https://bakerstreetforensics.com/wp-content/uploads/2020/11/week-5-flag.png?w=1024)

The block ID we're looking for is shown as part of a file copy operation. If we drop back about 10 lines to <PATH> we see that the filename of the file was **AptSource**.

![](https://bakerstreetforensics.com/wp-content/uploads/2020/11/89bc2fcc-a3ec-4ab2-adc7-6ae112190b99_1_201_a.jpeg?w=600)

Another week down.

Another challenge completed.

Another (multiple) things learned that I didn't know when I started.
