---
layout: post
permalink: http://bakerstreetforensics.com/2022/09/10/mal-hash-interacting-with-virus-total-api-via-powershell/
title: Mal-Hash - interacting with Virus Total API via PowerShell
description: None
date: 2022-09-10 12:14:32 -0000
last_modified_at: 2022-09-10 13:18:29 -0000
publish: true
pin: false
categories:
- DFIR
- PowerShell
tags:
- Github
- Malware
- Virus Total
---
Virus Total started in 2004 as a free service to analyze files and URLs for malicious behavior. In 2012 Virus Total (VT) was acquired by Google. Virus Total can provide a boon of information for the nascent investigator, though OpSec should remain a concern. 

It’s rare to be in a security class where Virus Total is mentioned and not be warned about submitting the file hash vs. submitting the file itself. Often the suspect file, (i.e. ‘companyXYZ_invoice.doc) could contain information that has been customized to the target, you or your company. You’d don’t need to be a big-game target. Often these files are distributed like mass marketing. The copy YOU receive may have information that traces back to you. The bad guys use Virus Total too you see - and if they see that companyXYZ_invoice.doc was submitted (or companyABC_invoice.doc, company123… etc.), it could tip them off as to who is on to them.

The preferred method of submission is to use the file hash. This value is unique* (insert debate about MD5 hash collisions) to the file and is safer to use as a reference to search for. Virus Total supports MD5, SHA1 and SHA256 hashes for lookup.

Virus Total has both free and Enterprise plans available. Registration gives you access to an API key that you can use to interact with VT. The free accounts are limited in the number of API queries you can submit. If you’re working on a project at enterprise scale, chances are you’ll need the license to do so to support the number of queries. 

**Mal-Hash** is a PowerShell script that utilizes the Virus Total API to interact with VT from the command-line. Your API key is kept in a file separate from the script. When you invoke the script, you point it to a file to analyze. 

![](https://bakerstreetforensics.com/wp-content/uploads/2022/09/screenshot-2022-09-10-at-8.52.09-am.png?w=1024)_Mal-Hash.ps1_

You can either type the path in manually or you can drag and drop the file onto the PowerShell window and the path will auto populate. 

![](https://bakerstreetforensics.com/wp-content/uploads/2022/09/screenshot-2022-09-10-at-8.52.52-am-1.png?w=1024)_Path of file to analyze_

The script uses the _Get-FileHash_ PowerShell command to get the MD5, SHA1 and SHA256 hash of the file. The script then (referencing your API key for the lookup), submits the MD5 (by default) hash to Virus Total. The results of the query are displayed back to the PowerShell instance and are also recorded to a text file.

![](https://bakerstreetforensics.com/wp-content/uploads/2022/09/screenshot-2022-09-10-at-8.56.05-am-1.png?w=1024) ![](https://bakerstreetforensics.com/wp-content/uploads/2022/09/screenshot-2022-09-10-at-8.55.01-am-1.png?w=1024)

You can get Mal-Hash.ps1 from my GitHub [here](https://github.com/dwmetz/PSHero/blob/master/Mal-Hash.ps1). As always, feel free to fork the project and contribute back to the code. Learning is a constant process.
