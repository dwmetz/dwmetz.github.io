---
layout: post
permalink: http://bakerstreetforensics.com/2024/06/03/installing-the-latest-sift-workstation-in-wsl/
title: Installing the latest SIFT Workstation in WSL
description: None
date: 2024-06-03 16:08:48 -0000
last_modified_at: 2024-06-03 18:55:10 -0000
publish: true
pin: false
image:
  path: https://bakerstreetforensics.com/wp-content/uploads/2024/06/image-2.png
categories:
- DFIR
- Forensic Imaging
tags:
- cast
- Forensics
- Github
- SIFT Workstation
- WSL
---
If you're like me and have your favorite forensic tools for Linux, and your favorite tools for Windows, you can run them both on the same machine without having to diminish resources with the use of a virtual machine. You can do this by installing SIFT (SANS Investigative Forensic Toolkit) within WSL (Windows Subsystem for Linux).

_Note: this article assumes that WSL is already installed. If not,[GTS](https://googlethatforyou.com?q=How%20to%20install%20WSL%20on%20windows)._

Start off by grabbing Ubuntu 22.04 from the Windows store, or if you prefer the command line. 
    
    
    wsl --install -d Ubuntu-22.04

![](https://bakerstreetforensics.com/wp-content/uploads/2024/06/screenshot-2024-06-03-at-9.55.19e280afam.png?w=1024) ![](https://bakerstreetforensics.com/wp-content/uploads/2024/06/screenshot-2024-06-03-at-9.57.35e280afam.png?w=1024)

New UNIX username: sansforensics

![](https://bakerstreetforensics.com/wp-content/uploads/2024/06/screenshot-2024-06-03-at-9.57.57e280afam.png?w=1024)

Password: ***************

Retype new password: ***************

![](https://bakerstreetforensics.com/wp-content/uploads/2024/06/screenshot-2024-06-03-at-9.58.41e280afam.png?w=1024)

Download **cast** from GitHub. 
    
    
    wget https://github.com/ekristen/cast/releases/download/v0.14.30/cast-v0.14.30-linux-amd64.deb

![](https://bakerstreetforensics.com/wp-content/uploads/2024/06/screenshot-2024-06-03-at-9.59.22e280afam.png?w=689)

Install cast from the download with the command
    
    
    sudo dpkg -i cast-v0.14.30-linux-amd64.deb

![](https://bakerstreetforensics.com/wp-content/uploads/2024/06/screenshot-2024-06-03-at-9.59.43e280afam.png?w=833)

Finally, install the **server mode** version of SIFT.  Server mode only installs the SIFT command line applications, which is most appropriate for running under WSL.
    
    
    sudo cast install --mode=server teamdfir/sift-saltstack

![](https://bakerstreetforensics.com/wp-content/uploads/2024/06/screenshot-2024-06-03-at-10.52.23e280afam.png?w=1024)

If all goes right you’ll see a wall of text that concludes (after a few minutes) with 'salt-call completed successfully.'

![](https://bakerstreetforensics.com/wp-content/uploads/2024/06/screenshot-2024-06-03-at-11.01.22e280afam.png?w=1024)

My go-to test for SIFT installations has always been to run Volatility (-h for help).
    
    
    vol.py -h

If you’re seeing output, the mission was a success.

![](https://bakerstreetforensics.com/wp-content/uploads/2024/06/screenshot-2024-06-03-at-11.17.35e280afam.png?w=1024)

Besides saving the resources needed for a full VM, you also don't have to worry about duplicating copies of evidence items as both Windows and Ubuntu are running on the same machine.

Now get yourself familiar with the Linux tools of the SIFT Workstation and enjoy running them in parallel with your favorite Windows forensic applications.

SIFT Cheat Sheet: <https://pentest.sans.org/security-resources/posters/sift-cheat-sheet/355/download>
