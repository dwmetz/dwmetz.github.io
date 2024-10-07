---
layout: post
permalink: http://bakerstreetforensics.com/2020/12/14/magnet-weekly-ctf-week-10-solution-walk-through/
title: 'Magnet Weekly CTF: Week 10 Solution Walk Through'
description: None
date: 2020-12-14 16:00:00 -0000
last_modified_at: 2020-12-13 20:22:37 -0000
publish: true
pin: false
categories:
- CTF
- DFIR
tags:
- Bulk_Extractor
- CTF; Magnet; DFIR; Memory; Volatility; REMnux
- Excel
- PCAP
---
This weeks challenge was another round of memory forensics. As with the previous weeks challenge most* of my solves were done using a REMnux VM. REMnux includes both Volatility 2.61 (SSL support deprecated) and the beta of Volatility 3. 

**_Challenge 10, Part 1: At the time of the RAM collection (20-Apr-20 23:23:26- Imageinfo) there was an established connection to a Google Server. What was the Remote IP address and port number? format: "xxx.xxx.xx.xxx:xxx"_**

Using the **netscan** plugin for Volatility and then filtering for "established" we see 4 open connections. A whois lookup on the addresses indicates that 172.253.188 is affiliated with Google. The communications were traversing port 443. 

![](https://bakerstreetforensics.com/wp-content/uploads/2020/12/flag-1-and-2.png?w=1024)Flag 1: **172.253.63.188:443**

**_Challenge 10, Part 2: What was the Local IP address and port number? same format as part 1_**

Looking at our earlier output from netscan we see the source address was **192.168.10.146:54282**

![](https://bakerstreetforensics.com/wp-content/uploads/2020/12/flag-2.png?w=1024)Flag 2: **192.168.10.146:54282**

**_Challenge 10, Part 3: What was the URL?_**

So we know we've got an open connection to a remote host going on at the time the memory was captured. Looking at Passive DNS for the Google IP address I could see that it's principally been associated with Google Chat related assets.

I ran the memory sample through Bulk Extractor to carve out a PCAP of any of the networking artifacts that were present in the image.

![](https://bakerstreetforensics.com/wp-content/uploads/2020/12/be-to-pcap.png?w=1024)bulk_extractor to pcap

Once the pcap was carved I opened it with Wireshark.

![](https://bakerstreetforensics.com/wp-content/uploads/2020/12/wireshark-open.png?w=1024)

Looking through the pcap we see that the client computer had received a DNS response for mtalk.google.com at 172.253.63.188

![](https://bakerstreetforensics.com/wp-content/uploads/2020/12/pcap.png?w=1024)

The question was looking for a URL so... **https://mtalk.google.com** \- yes! [Flag 3]. _Originally this answer wasn't accepted. I later received an email that it was accepted as an alternate solution and to resubmit_. 

**_Challenge 10, Part 4: What user was responsible for this activity based on the profile?_**

The **getsids** Volatility plugin quickly yields the active user account.

![](https://bakerstreetforensics.com/wp-content/uploads/2020/12/getsids.png?w=1024)Flag 4: **Warren**

**_Challenge 10, Part 5: How long was this user looking at this browser with this version of Chrome? *format: X:XX:XX.XXXXX * Hint: down to the last second_**

This one had me stumped for a while. I first tried calculating the delta between when the chrome process was started to the time the image was acquired. (Nope)

Eventually I succumbed and took the hint which had to do with "FOCUS". Another clue that I'd heard on Discord was that this was 'VERY easy to solve in Axiom.'

![](https://bakerstreetforensics.com/wp-content/uploads/2020/12/focus.png?w=1024)

Sure enough, stacking filters for Chrome and Focus and restricting the search to the Memory evidence yields a Registy artifact indicating that the focus time for Chrome was 3:36:47.301000. Oh but don't just copy paste here without checking the question for the flag format. There's only 5 places after the last decimal. So for Flag 5 the answer is **3:36:47.30100**.

I was victorious in getting all 5 flags, but this has been just as much about learning as it has been about the friendly competition. So now that I knew the answer and where relatively it could be found in the evidence, I wanted to see if I could solve it just with REMnux as I had the other challenge elements.

I ran the **timeliner** Volatility plugin and exported the output file to .xlsx. format.

![](https://bakerstreetforensics.com/wp-content/uploads/2020/12/timeliner.png?w=1024)

This took a few minutes to process. Once complete, I opened timeliner.xlsx.

![](https://bakerstreetforensics.com/wp-content/uploads/2020/12/flag5-excel.png?w=1024)Flag 5 (alternate) 3:36:47.30100

If we filter the **Item** column for _Chrome_ and the **Details** column for _Focus_ \- there again the same result - **3:36:47.30100**. _Don't forget to drop the last zero for the Flag._

![](https://bakerstreetforensics.com/wp-content/uploads/2020/12/bbc069ed-94f0-4546-8f55-bc28a38c84a8.jpeg?w=1024)
