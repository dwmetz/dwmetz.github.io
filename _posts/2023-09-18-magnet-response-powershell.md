---
layout: post
permalink: http://bakerstreetforensics.com/2023/09/18/magnet-response-powershell/
title: Magnet RESPONSE PowerShell
description: None
date: 2023-09-18 20:53:13 -0000
last_modified_at: 2023-09-19 11:55:35 -0000
publish: true
pin: false
categories:
- DFIR
- Forensic Imaging
- PowerShell
- Presentations
- RAM
tags:
- Automation
- Axiom
- Forensics
- Github
- Imaging
- Magnet
- Memory
- Network
- Triage
---
I’m excited to share with you a new script I’ve written, Magnet RESPONSE PowerShell.

[Magnet RESPONSE](https://www.magnetforensics.com/resources/magnet-response/) is a free tool from Magnet Forensics that makes it easy for investigators as well as non-technical operators to collect triage collections quickly and consistently.

Released initially as a GUI tool for law-enforcement investigators, it’s a single executable that requires no installation. The available command line syntax also makes it very flexible for enterprise use.

So what do I do when there’s a command line interface available, I PowerShell the hell out of it.

If you’ve been following my [CyberPipe](https://github.com/dwmetz/CyberPipe) project, you’ll definitely want to check this one out.

### MagnetRESPONSEPowerShell.ps1

[![](https://bakerstreetforensics.com/wp-content/uploads/2023/09/image.png)](https://github.com/MagnetForensics/Magnet-RESPONSE-PowerShell/blob/main/screenshot.png)

##### Functions:

  * 💻 Capture specified triage artifacts using profiles with Magnet RESPONSE,
  * 🐏 Capture a memory image with DumpIt for Windows or Magnet RAM Capture,
  * 💾 Save all artifacts, output, and audit logs to network drive.
  * 🪟 Supports x86, x64 and ARM64 versions of Windows



##### Prerequisites:

>   * [Magnet RESPONSE](https://www.magnetforensics.com/resources/magnet-response/)
>   * Web server where you can host MagnetRESPONSE.zip that’s accessible to endpoints.
>   * File server repository to save the file collections to.
> 


**Please note this is not a Magnet supported product. This script is open source. If you have comments, updates, or suggestions - please do so here or on GitHub via discussion or pull request.**

* * *

There are two areas of the script for you to customize.

>   * The _**Variable Setup**_  contains the case identification, file server and web server locations.
>   * The second section, _**Collection Profiles**_ , define which artifact groups you want to collect. You can see all the options available in the [Magnet RESPONSE CLI Guide](https://github.com/MagnetForensics/Magnet-RESPONSE-PowerShell/blob/main/Magnet_RESPONSE_CLI_Guide.pdf).
> 


### VARIABLE SETUP

`$caseID = "demo-161" # no spaces`

`$outputpath = "\\Server\Share" # Update to reflect output destination.`

`$server = "192.168.4.187" # "192.168.1.10" resolves to http://192.168.1.10/MagnetRESPONSE.zip`

### COLLECTION PROFILES

Within the script we need to have at least one set of collection arguments defined. In this case I’ve built multiple profiles, which are simply un-commented to mark the profile as active. You only want to have one profile enabled at a time. You can design your own collection profiles using any of the available CLI options, just follow the format below.

`#### Extended Process Capture`

`$profileName = "EXTENDED PROCESS CAPTURE"`

`$arguments = "/capturevolatile /captureextendedprocessinfo /saveprocfiles"`

### Execution

Once your environment and collection variables are defined, go ahead and run the script on your endpoints. Every host that executes the script will download RESPONSE from the web server, run the specified collection profile, and then save the output to the file server. All data defined in the collection profile will be collected and organized by case name, hostname and timestamp of collection in the central location. The returned files can be examined manually, using open source tools, or products like Magnet AXIOM Cyber.

If you’d like to learn more about the script, and how I integrated it with AXIOM Cyber and Magnet AUTOMATE, you can **register for my webcast,[Responding at Scale with Magnet RESPONSE.](https://www.magnetforensics.com/resources/responding-at-scale-with-magnet-response/)** I hope to see you there. 

You can download the script at <https://github.com/MagnetForensics/Magnet-RESPONSE-PowerShell>
