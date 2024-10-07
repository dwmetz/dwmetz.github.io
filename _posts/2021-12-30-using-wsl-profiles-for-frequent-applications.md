---
layout: post
permalink: http://bakerstreetforensics.com/2021/12/30/using-wsl-profiles-for-frequent-applications/
title: Using WSL Profiles for Frequent Applications
description: None
date: 2021-12-30 18:59:08 -0000
last_modified_at: 2022-01-17 03:32:19 -0000
publish: true
pin: false
categories:
- DFIR
- PowerShell
tags:
- Linux
- Volatility
- WSL
---
Windows Subsystem for Linux (WSL) adds a lot of capability and convenience for running DFIR applications on a Windows host. Previously I wrote about how to [add a SIFT/REMnux Ubuntu distribution to WSL](https://bakerstreetforensics.com/2021/05/26/adding-sift-and-remnux-to-your-windows-forensics-environment/). 

Another tip I'd like to share with you is setting up separate profiles for frequently used applications.

![](https://bakerstreetforensics.com/wp-content/uploads/2021/12/wsl-menu.png?w=1024)

Volatility is one of the applications I'm in frequently, whether for work or lab(work). Sure, I can open a command window and then navigate to the appropriate application path; but why not make it a one-click option.

To begin, open Windows Terminal, and go to the Settings menu.

On the bottom left choose select 'Add a new profile.'

![](https://bakerstreetforensics.com/wp-content/uploads/2021/12/add-profile.png?w=1024)

PowerShell (Core) is my default shell environment. I'll select this as the profile to duplicate.

After you hit 'Duplicate' you'll be presented with a copy of the profile.

![](https://bakerstreetforensics.com/wp-content/uploads/2021/12/copy-config.png?w=583)

Update the Name and Starting directory to reflect the application path.

You can customize the Icon and Tab title. Under the Appearance tab you can assign a custom background for the WSL profile. Be sure to click Save when you've made your changes.

![](https://bakerstreetforensics.com/wp-content/uploads/2021/12/volatility3-wsl.png?w=1024)

Now when I want to open a Volatility session, it's right there on the drop down in WSL.

![](https://bakerstreetforensics.com/wp-content/uploads/2021/12/vol-dash-h.png?w=1024)

If you have WSL parked on the Taskbar, you can select the new profile (or any other profile) with a right-click. 

![](https://bakerstreetforensics.com/wp-content/uploads/2021/12/wsl-task-menu.png?w=391)

If you want to have your WSL instances in separate windows, versus the default tabbed layout, right clicking from the taskbar will open the selected session in a new window.
