---
layout: post
permalink: http://bakerstreetforensics.com/2021/02/09/getting-started-with-a-powershell-menu/
title: Getting Started with a PowerShell Menu
description: None
date: 2021-02-09 16:19:50 -0000
last_modified_at: 2021-02-09 16:19:50 -0000
publish: true
pin: false
categories:
- DFIR
tags:
- Automation
- Github
- PowerShell
---
We're often using PowerShell within the Incident Response team. I'm a big practitioner of spending 5 hours coding something to automate a 5 minute job. At first the math may not compute, but when that 5 minute job may be requested hundreds of times - and with it scripted it takes 30 seconds… that's where it pays off. It also enforces consistency and removes some of the possibility for human error.

We have a collection of internal scripts that we use frequently. As more scripts (or _scriptlets_) are added to the frequently used, I wanted a means to expose all the scripts to the team and to put some organization to it. I also wanted to easily support changes or additions to the referenced scripts. What I wound up building was a simple PowerShell menu structure.

![](https://bakerstreetforensics.com/wp-content/uploads/2021/02/screenshot.png?w=1024)

Each individual script is referenced by a 2 letter code in the menu. Right now in our environment there's 38 scripts in the menu. Many of those are proprietary (can't share), however I gathered a handful to share here to illustrate the concept of the menu process.

https://github.com/dwmetz/PSHero

Once you've downloaded and unzipped the repository, you'll want to edit the PSHero.ps1 file to ensure that the paths for the scripts reflect where you've got them saved to.

![](https://bakerstreetforensics.com/wp-content/uploads/2021/02/screen-shot-2021-02-09-at-8.49.53-am.png?w=1024)

To add or remove scripts from the menu, there are 2 modifications:  
In the top section is the menu listing

`**Write-Host "EX: MX Header Analysis"**`

Which pairs with

`**'EX' {  
D:\PowerShell\PSHero\Parse-EmailHeader.ps1  
}**`

in the lower section. Use the other scripts as examples and add what you like. _Just watch your brackets._

The scripts included in this demo menu include:

  * Launch PowerShell with an alternate credential
  * Login to O365, Legacy and Modern Auth.
  * Bitlocker (AD) retrieval
  * Host profiling and an _attention getting PING_
  * IRMemPull (Memory Acquisition) - https://github.com/n3l5/irMempull
  * A script that adds an examiner permissions to subject mailbox (for Magnet Axiom collection)
  * MX header parsing
  * Sadphishes - https://github.com/EdwardsCP/powershell-scripts/blob/master/SADPhishes.ps1
  * Convert Unix time to human readable



Have a favorite PS script you use? Post a recommendation in the comments below.
