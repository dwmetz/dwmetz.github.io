---
layout: post
permalink: http://bakerstreetforensics.com/2022/07/18/axiom-yara-github-oh-my/
title: AXIOM, YARA, GitHub - Oh My!
description: None
date: 2022-07-18 17:18:08 -0000
last_modified_at: 2022-07-18 19:32:28 -0000
publish: true
pin: false
categories:
- DFIR
- PowerShell
- yara
tags:
- Axiom
- Github
- ReversingLabs
---
Version 6 of Magnet Axiom [added support for YARA rules](https://mfistaging.magnetforensics.com/blog/magnet-axiom-cyber-6-0-yara-rules-queued-collections-dark-mode-and-more/). By default the installation ships with the free Open-Source YARA rules from [Reversing Labs](https://www.reversinglabs.com/products/open-source-yara-rules). These YARA rules may be updated within Axiom periodically. In addition to the included rules, AXIOM supports adding your own YARA source folders. 

If you need to update the included rules on demand, you can do so with a PowerShell script and the GitHub CLI. The script below can be used to update the included rules, as well as other YARA sources you may be using within Axiom. 

![](https://bakerstreetforensics.com/wp-content/uploads/2022/07/yara.png?w=1024)

**Prerequisites:**

  * Prior to running the script you'll need to install [GitHub CLI](https://cli.github.com/)
  * Once installed run `**gh auth login** `to establish authentication with GitHub
  * When running the script you will need to run as an Administrator in order for the file-copy to ~\ProgramFiles to be successful



### Set the working directory to the local git repository for the YARA rules

`Set-Location C:\GitHub\reversinglabs-yara-rules\`

### Sync the repository; requires github CLI https://cli.github.com/

`gh repo sync`

### Create local archive directory

`mkdir C:\Archives -Force`

### Backup the existing YARA rules in Axiom

`Get-ChildItem -Path "C:\Program Files\Magnet Forensics\Magnet AXIOM\YARA" | Compress-Archive -DestinationPath C:\Archives\AxiomYARA.zip`

### Variable for date/time

`$timestamp = Get-Date -Format o | ForEach-Object { $_ -replace ":", "." }`

### Set the working directory to the Archives location

`Set-Location "C:\Archives"`

### Rename the archive with timestamp

`Get-ChildItem -Filter '_AxiomYARA_ ' -Recurse | Rename-Item -NewName {$_.name -replace 'AxiomYARA', $timestamp }`

### Copy new YARA rules to Axiom

`robocopy /s C:\GitHub\reversinglabs-yara-rules\yara "C:\Program Files\Magnet Forensics\Magnet AXIOM\YARA\ReversingLabs"`

* * *

### **Now let's run it all together in a single script:**

`Set-Location C:\GitHub\reversinglabs-yara-rules\  
gh repo sync  
mkdir C:\Archives -Force  
Get-ChildItem -Path "C:\Program Files\Magnet Forensics\Magnet AXIOM\YARA" | Compress-Archive -DestinationPath C:\Archives\AxiomYARA.zip  
$timestamp = Get-Date -Format o | ForEach-Object { $_ -replace ":", "." }  
Set-Location "C:\Archives"  
Get-ChildItem -Filter '_AxiomYARA_ ' -Recurse | Rename-Item -NewName {$_.name -replace 'AxiomYARA', $timestamp }  
robocopy /s C:\GitHub\reversinglabs-yara-rules\yara "C:\Program Files\Magnet Forensics\Magnet AXIOM\YARA\ReversingLabs"`

* * *

That's all there is to it. If you've got multiple repositories to sync, just add lines to _cd_(Set-Location) into those directories and repeat the `gh repo sync `command.

Feel free to copy the code above, or you can download directly from my [GitHub](https://github.com/dwmetz/Axiom-PowerShell/blob/main/AX-YaraSync.ps1).

Are you utilizing YARA rules within AXIOM? If so, leave a comment on what are some that you've found useful.
