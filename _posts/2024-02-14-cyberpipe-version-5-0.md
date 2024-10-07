---
layout: post
permalink: http://bakerstreetforensics.com/2024/02/14/cyberpipe-version-5-0/
title: CyberPipe version 5.0
description: None
date: 2024-02-14 14:15:58 -0000
last_modified_at: 2024-02-14 14:15:58 -0000
publish: true
pin: false
categories:
- DFIR
- Forensic Imaging
- PowerShell
- RAM
- Triage
- USB
tags:
- Forensics
- Github
- Magnet
- Memory
---
The latest update to CyberPipe (_the code formerly known as CSIRT-Collect),_ has been revised to leverage the free triage collection tool, MAGNET Response. As with previous versions it also runs Encrypted Disk Detector, another [free tool](https://www.magnetforensics.com/free-tools/) from MAGNET.

![](https://bakerstreetforensics.com/wp-content/uploads/2024/02/screenshot.png?w=1024)

## Script Functions:

  * Capture a memory image with MAGNET DumpIt for Windows, (x32, x64, ARM64), or MAGNET RAM Capture on legacy systems,
  * Create a Triage collection* with MAGNET Response,
  * Check for encrypted disks with Encrypted Disk Detector,
  * Recover the active BitLocker Recovery key,
  * Save all artifacts, output and audit logs to USB or source network drive.



* There are collection profiles available for: 

  * Volatile Artifacts
  * Triage Collection (Volatile, RAM, Pagefile, Triage artifacts)
  * Just RAM
  * RAM & Pagefile
  * _or build your own using the RESPONSE CLI options_



## Prerequisites:

The setup is simple. Save the CyberPipe script to a **USB drive**. Next to the script is a **Tools** folder with the **executables for MAGNET Response & EDD**. Before running, customize the script to **select a collection profile**. Run the script from the USB drive and collect away. Move on to the next PC and run it again.

## Network Usage:

CyberPipe 5 also has the capability to write captures to a network repository. Just un-comment the `**# Network section**` and update the `**\\server\share**` line to reflect your environment.

In this configuration it can be included as part of automation functions like a collection being triggered from an event logged on the EDR.

## Prior Version (KAPE Support):

If you're a prior user of CyberPipe and want to use the previous method where KAPE facilitates the collection with the MAGNET tools, or have made other KAPE modifications, use v4.01.

## Download:

Download the latest release of CyberPIpe on [GitHub](https://github.com/dwmetz/CyberPipe).

https://github.com/dwmetz/CyberPipe
