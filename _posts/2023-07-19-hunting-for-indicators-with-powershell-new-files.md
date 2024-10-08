---
layout: post
permalink: http://bakerstreetforensics.com/2023/07/19/hunting-for-indicators-with-powershell-new-files/
title: 'Hunting for Indicators with PowerShell: New Files'
description: None
date: 2023-07-19 21:04:33 -0000
last_modified_at: 2023-07-20 11:59:00 -0000
publish: true
pin: false
categories:
- DFIR
- PowerShell
tags:
- Malware
---
When analyzing the impact of malware execution on a system, it's important to identify what additional files the malware has introduced to the system. Have other exe's been dropped? Are there .vbs files being sprinkled around by the malware fairies? 

What other file types would you be concerned with showing up on your systems?

Maybe it's the inverse and it's the file extension itself that's the outlier and you need to identify all the _.m41z_ files, as an example.

![](https://bakerstreetforensics.com/wp-content/uploads/2023/07/screenshot-2023-07-19-at-11.59.17e280afam.png?w=462)

I wanted an easy way to identify new files on the system, and yet be flexible to incorporate different extensions and durations. As usual, a PowerShell script seemed the easiest way to address it.

There are 2 inputs, file extension, and duration. What are the _**kind**_ of new files are you looking for and _**how far back**_ do you want to look?
    
    
    <#
    
    GetNewFiles.ps1
    @dwmetz, 19-july-2023
    A simple script to find any new files on the file system for a specific filetype within x # of days
    
    #>
    Write-host " "
    $script:filetype = Read-host -Prompt 'Enter the file type to look for (ex. txt, ps1, exe)'
    $script:time = Read-host -Prompt 'How many days back do you want to look?'
    $ErrorActionPreference = "SilentlyContinue"
    Write-host " "
    $NewFiles = Get-ChildItem -Path c:\ -Recurse  -Filter "*.$script:filetype" |
    Where-Object { $_.CreationTime -gt (Get-Date).AddDays(-$script:time) }
    "Number of $script:filetype files found: $($NewFiles.Count)"
    $NewFiles | Select-Object Fullname,CreationTime
    

Running the script on a suspected infected asset, I can look for new files of interest and if need be work backwards for a larger time sampling. 

![](https://bakerstreetforensics.com/wp-content/uploads/2023/07/screenshot-2023-07-19-at-4.57.56e280afpm.png?w=1024)

In the example above, one of the executables bears looking into. The other is benign and related to software updates.

For the .ps1 results we see the script we're running "GetNewFiles.ps1" as well as another hit for a script that was created on the system the day before.

NOTE: If a file is no longer there when you run the script, for example created and deleted during the malware operation, you won't see it here as it's no longer present on the system.

If you run the malware on multiple samples, can you see a commonality among the new files? Does it always drop '_notAsafeFILE.exe_ ' in the same path, or is there a randomization in the file naming and location? PowerShell can be a quick way to come to that answer and identify what other files require investigation.
