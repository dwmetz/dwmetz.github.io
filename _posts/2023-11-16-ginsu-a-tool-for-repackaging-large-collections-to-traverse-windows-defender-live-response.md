---
layout: post
permalink: http://bakerstreetforensics.com/2023/11/16/ginsu-a-tool-for-repackaging-large-collections-to-traverse-windows-defender-live-response/
title: 'Ginsu: A tool for repackaging large collections to traverse Windows Defender
  Live Response'
description: None
date: 2023-11-16 18:40:26 -0000
last_modified_at: 2023-11-19 14:24:26 -0000
publish: true
pin: false
categories:
- DFIR
- M365
- PowerShell
- Triage
tags:
- 7zip
- Defender
- Forensics
- Free Tools
- Ginsu
- Github
- Imaging
- KAPE
- Magnet RESPONSE
---
![](https://bakerstreetforensics.com/wp-content/uploads/2023/11/img_9100-1.png?w=1024)_Screenshot of Ginsu.ps1_

Enterprise customers running Windows Defender for Endpoint have a lot of capability at their fingertips. This includes the Live Response console, a limited command shell to interact with any managed Defender assets that are online. Besides its native commands you can also use the console to push scripts and executables to endpoints. 

Note: there is a specific security setting in the Defender console if you want to allow unsigned scripts. 

![](https://bakerstreetforensics.com/wp-content/uploads/2023/11/img_9099-1.jpg?w=1024)

Microsoft has its own triage package capability, but you can also push your own tools like Magnet RESPONSE or KAPE. With a little bit of PowerShell mojo you can use your favorite collection utilities using the Defender Live Response console as your entry point into the remote asset. 

The console enables you to pull back files from the remote endpoint, even when it’s been quarantined. One limitation of this console function is that you’re limited to retrieving files of 3GB or less. 

For many triage collections this could be under the limit, but depending on the artifacts you’re collecting you might exceed that. So what do you do when you have an isolated endpoint but you need to pull back files over 3GB? That’s where **Ginsu** comes in. 

Ginsu is a PowerShell script that you can upload to your Defender console along with the command line version of 7zip. You configure the script with the directory with the contents you want to transfer. The script acts as a wrapper for 7zip and will create a multipart archive, splitting the files into 3GB segments. 

Once you pull the archives back to your workstation, you can use 7zip to extract the files back into their original properties. 

In testing, the file transfer capabilities were a bit buggy, whether it was transferring 3GB Ginsu files or other smaller files from the asset. I’m hoping this improves as the Defender console matures. If you’re able to text Ginsu in your environment, I’d love to hear how it performs. 

You can download Ginsu from my GitHub repo at <https://github.com/dwmetz/Ginsu>
