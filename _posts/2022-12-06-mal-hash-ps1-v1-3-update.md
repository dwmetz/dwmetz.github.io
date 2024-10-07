---
layout: post
permalink: http://bakerstreetforensics.com/2022/12/06/mal-hash-ps1-v1-3-update/
title: Mal-Hash.ps1 (v1.3 Update)
description: None
date: 2022-12-06 20:51:51 -0000
last_modified_at: 2022-12-06 20:51:51 -0000
publish: true
pin: false
categories:
- DFIR
tags: []
---
I've made some updates to the Mal-Hash PowerShell script. Most notable is that the script now works (via PowerShell) on **Windows, Mac and Linux**.

![Mal-Hash.ps1 output displayed on Linux \(REMnux\), Windows & MacOS.](https://bakerstreetforensics.com/wp-content/uploads/2022/12/mal-hash-cross-platform.png?w=1024)

The script takes the input of a file, calculates the hashes (MD5, SHA1, SHA256), and then submits the **_HASH_** to Virus Total for analysis. The script will also run Strings against the sample. The hashes, strings and Virus Total results are both displayed on screen and saved to a text report. Timestamp of the analysis is recorded in UTC.

Get Mal-Hash.ps1 at [GitHub](https://github.com/dwmetz/Mal-Hash/)
