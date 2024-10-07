---
layout: post
permalink: http://bakerstreetforensics.com/2021/12/17/csirt-collect-usb/
title: CSIRT-Collect USB
description: None
date: 2021-12-17 14:05:11 -0000
last_modified_at: 2021-12-17 14:07:15 -0000
publish: true
pin: false
categories:
- DFIR
- Forensic Imaging
- PowerShell
- RAM
- USB
tags:
- Free Tools
- Imaging
- KAPE
- Magnet
- Memory
---
CSIRT-Collect USB can be found in the main repository for CSIRT-Collect. CSIRT-Collect is a PowerShell script to collect memory and (triage) disk forensics for incident response investigations.

![](https://bakerstreetforensics.com/wp-content/uploads/2021/12/usb-.png?w=572)

**CSIRT-Collect USB** is designed to run directly from a USB device. While a network deployment certainly supports automation, as an Incident Responder I can think of several examples where that wouldn't be an option:

  * An air-gapped manufacturing environment
  * Hospital/Medical Environments
  * Ransomware incidents when the assets have been detached from the network



**Preparation** is the first phase of the Incident Response lifecycle. (PICERL) Once you've tested and/or adapted the collection for your environment, consider prepping a handful of drives and having them pre-deployed to sites where you're likely to need them.

![](https://bakerstreetforensics.com/wp-content/uploads/2021/12/20211217_123726000_ios.jpg?w=758)

## **The Setup**

First off you're going to need a high-capacity USB device. Larger sized flash drives will work. Personally I'm a fan of Samsung (T series) SSD drives, both for their size and their write speeds during acquisitions.

On the root of the USB device:

  * A (initially empty) folder named 'Collections'
  * KAPE directory from default KAPE installation
  * EDD.exe in \KAPE\Modules\bin\EDD (Encrypted Disk Detector)
  * CSIRT-Collect_USB.ps1
  * MRC.exe (Magnet RAM Capture)

![](https://bakerstreetforensics.com/wp-content/uploads/2021/12/setup.png?w=892)

## **Launch**

To run the script, open an elevated PowerShell prompt and browse to the USB device. Then simply
    
    
    .\CSIRT-Collect_USB.ps1

![](https://bakerstreetforensics.com/wp-content/uploads/2021/12/start.png?w=1003)CSIRT-Collect_USB.ps1 starting

## What it Captures

The first process the script runs is Magnet RAM Capture. Once the RAM has been captured, the windows build (profile) is captured. The RAM image and the build info are named to reflect the asset hostname being collected.

The next process is the KAPE Triage collection. Host artifacts are acquired and then assembled as a .vhdx (portable hard disk) image. After the KAPE _Targets_ portion completes, KAPE calls the Encrypted Disk Detector module which checks the local physical drives on a system for TrueCrypt, PGP, VeraCrypt, SafeBoot, or Bitlocker encrypted volumes. This information is saved into the Collections directory, as well as displayed to the responder to identify other volumes that may need to be collected while the system is live.

Lastly, if BitLocker is enabled for the OS drive the script will capture that information as well and back-up the recovery key.

![](https://bakerstreetforensics.com/wp-content/uploads/2021/12/encryption-check.png?w=1024)Disk Encryption Check

## Collection Contents

Inside the Collections folder, a subfolder will be created for each asset collected. The size of the USB device will determine how many collections can be captured before the results need to be offloaded.

The **\Collections\%hostname%** directory will include:

  * Console log capturing all KAPE targets activity
  * .vhdx of the host artifacts
  * collection complete date/time .txt
  * Memory acquisition .raw
  * Windows profile (build information) .txt

![](https://bakerstreetforensics.com/wp-content/uploads/2021/12/collection-contents.png?w=1024)

In the**\Collections\%hostname%\Decrypt** folder you will find

  * console log for KAPE modules (EDD)
  * recovery key for BitLocker (C) volume .txt
  *  _**Live Response**_ directory with the output of EDD .txt



###

https://github.com/dwmetz/CSIRT-Collect

###
