---
layout: post
permalink: http://bakerstreetforensics.com/2021/02/22/csirt-collect/
title: CSIRT-Collect
description: None
date: 2021-02-22 17:51:47 -0000
last_modified_at: 2021-02-22 17:56:51 -0000
publish: true
pin: false
categories:
- DFIR
- Forensic Imaging
- PowerShell
tags:
- Arsenal Image Mounter
- Axiom
- Enterprise Pulse
- Forensics
- Imaging
- KAPE
- Magnet
- Memory
---
**A PowerShell script to collect memory and (triage) disk forensics for incident response investigations**

There's a number of tools that support a one-to-many remote operation capability. However, not all organizations have that level of capability. I've also seen that in some large organizations how things are designed to work with remote assets, and how they actually work, may not be the same. What I wanted was a repeatable pre-defined collection mechanism, that could scale out to be supported by non-forensics team members to participate in forensic evidence collection for incident response examinations. The intent is that the collection process can be distributed among remote team members, be it site support or Security Operations Center (SOC). The script can also be integrated into SOAR and EDR platforms.

CSIRT-Collect was written to fill that role.

https://github.com/dwmetz/CSIRT-Collect

![](https://bakerstreetforensics.com/wp-content/uploads/2021/02/picture1.png?w=1024)

**CSIRT-Collect leverages a network share, from which it will access and copy the required executables and subsequently upload the acquired evidence to the same share post-collection.**

Permission requirements for said directory will be dependent on the nuances of the environment and what credentials are used for the script execution (interactive vs. automation). In the demonstration code, a network location of \\\Synology\Collections can be seen. This should be changed to reflect the specifics of your environment.

The Collections folder will need to include:  
\- subdirectory KAPE; copy the directory from any existing install  
\- subdirectory MEMORY; 7za.exe command line version of 7zip and winpmem.exe

## CSIRT-Collect Operations:

  * Maps to existing network drive -
  * Subdir 1: “Memory” – Winpmem and 7zip executables
  * Subdir 2: ”KAPE” – directory (copied from local install)
  * Creates a local directory on asset
  * Copies the Memory exe files to local directory
  * Captures memory with Winpmem
  * When complete, ZIPs the memory image
  * Renames the zip file based on hostname
  * Documents the OS Build Info (no need to determine profile for Volatility)
  * Compressed image is copied to network directory and deleted from host after transfer complete
  * New temp Directory on asset for KAPE output
  * KAPE !SANS_Triage collection is run using VHDX as output format [$hostname.vhdx] **
  * VHDX transfers to network
  * Removes the local KAPE directory after completion
  * Writes a “Process complete” text file to network to signal investigators that collection is ready for analysis.



_** Note: you can build different KAPE collection profiles by modifying just one line of code. Profiles can be chosen to support the requirements of the investigation._

## CSIRT-Collect_USB

This is a separate script that performs essentially the same functionality as CSIRT-Collect.ps1 with the exception that it is intended to be run from a USB device. There is no need for a temporary host directory as the information is written direct to the USB device. The extra compression operations on the memory image and KAPE .vhdx have also been omitted. There is a slight change noted below to the folder structure for the USB version. On the root of the USB:

  * CSIRT-Collect_USB.ps1
  * folder (empty to start) titled 'Collections'
  * folders for KAPE and Memory - same as above
  * Execution: -Open PowerShell as Adminstrator -Navigate to the USB device -Execute ./CSIRT-Collect_USB.ps1



To see a demonstration of CSIRT-Collect in action please register for my talk this Thursday, **PowerShell Tools for IR Forensics Collection** as part of the [Enterprise Pulse](https://magnetforensics.swoogo.com/enterprise-pulse) lecture series hosted by Magnet Forensics. 

[![](https://bakerstreetforensics.com/wp-content/uploads/2021/02/enterprise-pulse.png?w=1024)](https://magnetforensics.swoogo.com/enterprise-pulse)

Q&A will be live on [Discord](https://discord.gg/fMkHuYybSb) during the event.
