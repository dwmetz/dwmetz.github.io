---
layout: post
permalink: http://bakerstreetforensics.com/2021/05/26/adding-sift-and-remnux-to-your-windows-forensics-environment/
title: Adding SIFT and REMnux to your Windows Forensics environment
description: None
date: 2021-05-26 14:11:04 -0000
last_modified_at: 2022-01-18 16:06:18 -0000
publish: true
pin: false
categories:
- DFIR
tags:
- PowerShell
- REMnux
- SIFT
- WSL
---
![](https://bakerstreetforensics.com/wp-content/uploads/2021/05/d59c341a-d3f0-43e4-8238-6b8bddf890a5-2.jpeg?w=1024)

I’ve been a fan of the SIFT Linux distribution from my very first SANS class. I think back then Ed Skoudis was teaching Nmap subnetting on an abacus, but still it’s been a loyal companion ever since. I've got an archive of all the distributions (with their class specific tweaks) from all the courses I've taken throughout my career. Recently, I've been using REMnux, another SANS Linux distribution, specifically for Volatility 3 for memory analysis and some of the other tools for malicious document examinations. 

Through all these years of use, it was almost all leveraging virtual machine (VM) images. Often there was at least one machine in my home lab that had SIFT running as the native OS - for when I had something processor or memory intensive to run. The challenge with VM's is that they're competing with the host system for resources. As Moore's Law has advanced so have the clock cycles at my disposal - but there's still always going to be a trade-off, so scale your systems appropriately.

A little over a year ago, I started using the "packages only" or "server mode" of the SIFT distribution, running under Windows Subsystem for Linux (WSL) on a Windows 10 machine. The installation wasn't always smooth but once it was running - good times. I now had all my favorite Linux forensics tools running side by side on my Windows system.

The SIFT distribution was recently updated with full support for the latest LTS version of Ubuntu (20.04). REMnux as a standalone has been on 20.04 for a while. What I'm going to walk you through here is **_how to install both SIFT and REMnux within WSL, and how to backup and share your customized install._**

Prerequisite 1: Up to date Windows 10 system.

Prerequisite 2: Install Windows Subsystem for Linux (WSL) <https://docs.microsoft.com/en-us/windows/wsl/install-win10>

Once WSL is enabled and you've done the reboot if required, go the the Microsoft Store and install the latest version of Ubuntu. <https://www.microsoft.com/en-us/p/ubuntu/9nblggh4msv6?activetab=pivot:overviewtab>

## Installing SIFT on WSL

On the first boot, Ubuntu will have you specify a username and password. Forensicator works for me and doesn't present any OpSec issues if I have to include screenshots in reports (or intriguing blog posts).

Before installing SIFT, ensure the OS is up to date by running **sudo apt update && sudo apt upgrade**

Elevate to root to for the installation, otherwise there may be permissions issues during the install. **sudo su**

Follow the instructions here to install the SIFT CLI (Command Line Interface): <https://github.com/teamdfir/sift-cli#installation>

Install SIFT within WSL using the syntax **sift install --mode=server**

_The process could take a while depending on both your hardware resources and internet speeds. Feel free to browse other posts here at Baker Street while you wait. Just make sure you come back as there's more to do._

## Adding REMnux to SIFT

Once the SIFT distribution is installed, we're going to add the REMnux distribution over the top. Doing so will provide you the full toolset of both distributions, all running in one WSL instance.

We'll use the process here to **Add [REMnux] to an Existing System** <https://docs.remnux.org/install-distro/add-to-existing-system>

Note: After the install, REMnux will suggest you reboot the pc. How do you reboot an Ubuntu instance in WSL? Open a PowerShell window as Administrator and type **Get-Service LxssManager | Restart-Service**

![](https://bakerstreetforensics.com/wp-content/uploads/2021/05/reboot.png?w=841)

**update - If you're running WSL 2, the command to 'reboot' WSL is 
    
    
    wsl --shutdown

When you launch the instance the next time wsl will start.

Great! Now we've got the full tool stack from both - running within our Windows environment. I prefer using WSL over a VM when I have the opportunity as the overhead resources used to run the 2nd OS (Ubuntu) in WSL is less intensive than booting up a full VM.

Just one more step and we'll be able to backup, copy and reload the customized build.

## Exporting your SIFT-REMnux Distro

Exporting your build will enable a number of things. If your environment gets corrupted for any reason you can reload the build from a known good state. You can also use this format to share the installation with members of your team so you're all working from the same toolset. This also works well to add this customized build to a system that may have restricted or limited internet access and cannot access all the necessary repositories to pull down the tools.

In a PowerShell window as Administrator, **wsl -l** will list the installed WSL distributions. In this case the only installation is the Ubuntu installation we just customized. 

![](https://bakerstreetforensics.com/wp-content/uploads/2021/05/list.png?w=526)

In this example I'm exporting the instance to a location on a D:\ drive with the filename of **SIFT-REMnux.tar**. The syntax is **wsl --export [name of WSL instance] [export file path and file name]**. _Tar is the required format for backing up and restoring WSL instances_.

![](https://bakerstreetforensics.com/wp-content/uploads/2021/05/export.png?w=845)

Voila! You've now got a backup of your SIFT and REMnux WSL installation. On the last step I'll show you have you can import the customized distro to another Windows 10 system.   


## Importing your SIFT-REMnux Distro

Note: the new system will need to have WSL enabled as discussed in the beginning of the post. The Ubuntu distro does NOT need to be installed.

To import the distro use the syntax **wsl --import [desired name for distro] [file path where distro will live] [tar file being imported].** In this case I have the .tar file in C:\WSL and will be installing to C:\WSL\SIFT-Linux folder. Once again you want to use an elevated PowerShell session to perform the import.

![](https://bakerstreetforensics.com/wp-content/uploads/2021/05/import-2.png?w=865)

That's it. You've now added the customized SIFT-REMnux WSL instance to your system. 

Once the process completes you can verify the distro was loaded using the **wsl -l** command

![](https://bakerstreetforensics.com/wp-content/uploads/2021/05/list2.png?w=529)In this case I had a previous Ubuntu 18.04 instance, and now the new SIFT-REMnux instance is visible as well.

To invoke your SIFT-REMnux instance **wsl --distribution SIFT-REMnux**

![](https://bakerstreetforensics.com/wp-content/uploads/2021/05/invoke2.png?w=1000)

To validate the running version numbers for SIFT and REMnux use **sift -v** and **remnux -v** respectively.

![](https://bakerstreetforensics.com/wp-content/uploads/2021/05/verify.png?w=697) ![](https://bakerstreetforensics.com/wp-content/uploads/2021/05/cheers-2.gif?w=245)

CHEERS!
