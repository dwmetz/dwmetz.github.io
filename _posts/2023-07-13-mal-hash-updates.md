---
layout: post
permalink: http://bakerstreetforensics.com/2023/07/13/mal-hash-updates/
title: Mal-Hash Updates
description: None
date: 2023-07-13 14:15:49 -0000
last_modified_at: 2023-07-13 14:15:49 -0000
publish: true
pin: false
image:
  path: https://bakerstreetforensics.com/wp-content/uploads/2023/07/screenshot-2023-07-13-at-9.58.14e280afam.png
categories:
- DFIR
- PowerShell
tags:
- API
- Malware
- Malware Bazaar
- VirusTotal
---
## Mal-Hash.ps1

  * The script takes the input of a file, calculates the hashes (MD5, SHA1, SHA256), and then submits the SHA256 hash to Virus Total* for analysis.
  * The script will also run Strings against the sample.
  * The script will check Malware Bazaar to see if a sample matching the hash is available.
  * The hashes, strings, Virus Total and Malware Bazaar results are both displayed on screen and saved to a text report.
  * Timestamp of the analysis is recorded in UTC.



## [](https://github.com/dwmetz/Mal-Hash#vthashsubps1)VTHashSub.ps1

  * The script takes a hash value as input and submits the hash to Virus Total* for analysis.
  * The script will check Malware Bazaar to see if a sample matching the hash is available.
  * The hashes, Virus Total and Malware Bazaar results are both displayed on screen and saved to a text report.
  * Timestamp of the analysis is recorded in UTC.



Mal-Hash.ps1 and VTHashSub.ps1 will operate (via PowerShell) on Windows, Mac & Linux.

_* Virus Total API key expected in vt-api.txt._

## [](https://github.com/dwmetz/Mal-Hash#latest-updates)Latest updates:

  * n of x vendors detected
  * VT permalink
  * Malware Bazaar results

![](https://bakerstreetforensics.com/wp-content/uploads/2023/07/screenshot-2023-07-13-at-9.58.14e280afam.png?w=1024) ![](https://bakerstreetforensics.com/wp-content/uploads/2023/07/screenshot-2023-07-13-at-10.01.05e280afam.png?w=1003) ![](https://bakerstreetforensics.com/wp-content/uploads/2023/07/screenshot-2023-07-13-at-9.59.27e280afam.png?w=1024) ![](https://bakerstreetforensics.com/wp-content/uploads/2023/07/screenshot-2023-07-13-at-9.58.39e280afam.png?w=1024)

Both scripts available on my GitHub page:

https://github.com/dwmetz/Mal-Hash
