---
layout: post
permalink: http://bakerstreetforensics.com/2023/01/16/kape-batch-mode-arm-memory-updates-to-csirt-collect-and-all-the-things-i-learned-along-the-way/
title: KAPE batch mode, ARM Memory, updates to CSIRT-Collect, and all the things I
  learned along the way.
description: None
date: 2023-01-16 22:09:12 -0000
last_modified_at: 2023-03-25 20:54:26 -0000
publish: true
pin: false
categories:
- DFIR
tags:
- ARM64
- Github
- Imaging
- KAPE
- Magnet
- Memory
- PowerShell
---
![](https://bakerstreetforensics.com/wp-content/uploads/2023/01/cyberpipe_arm.png?w=796)

A couple weeks ago, a reader commented on the post [Adding RAM collections to KAPE triage](https://bakerstreetforensics.com/2021/12/13/adding-ram-collections-to-kape-triage/), _"Couldn’t this be implemented by using linear processing with KAPE in batch mode?"_ and I couldn't be more grateful for their inquiry.

When I was first introduced to KAPE, 'batch mode' didn't exist yet as a feature. From the [documentation](https://ericzimmerman.github.io/KapeDocs/#!Pages%5C3.1-Batch-mode.md): 

_"Batch mode works by placing one or more command lines (without the  `kape.exe` part) into a file named `_kape.cli`. This file should contain one full command line on each line. This allows you to preconfigure KAPE to perform a given action (for example, collect certain files, zip them, then SFTP them to somewhere)."_

Essentially it allows you to string together multiple KAPE jobs and run them together. This could be useful when you want to send the output of one command to a network share, and another to S3. I'm sure there are many other use cases, but I haven't explored many as of yet. 

KAPE has supported memory collection from its very early days. In fact I wrote the original DumpIt plugin for KAPE back in May '19 (_Pepperidge Farm Remembers_). I later wrote the module for Magnet RAM Capture. Rounding out the triad is Winpmem which was added by another contributor around the same time (_hours apart if I remember_) as the DumpIt plugin.

At the time I thought I'd finally solved my Incident Responders ultimate dream of 'give me RAM and a selective triage image - and give it to me quickly.' While using any of the memory modules with KAPE, and an appropriate Targets selection for triage _would_ yield both results, to me there was still a problem. The way KAPE operates it initiates the Targets operations (collections) and then the Modules (execution) operations. If I selected a triage image and included the memory collection module, the triage collection would run first and after would run the memory collection (module). While I was pretty giddy over having made what would be my first public contributions to DFIR software, I really wanted to grab the memory first and _then_ do any other operations. The goal was to preserve as much volatile data as possible. It was out of that need to control the order of operations that led me to write [CSIRT-Collect](https://github.com/dwmetz/CSIRT-Collect/). 

Via PowerShell, CSIRT-Collect would collect a RAM image first (evolving over the years experimenting with DumpIt, Magnet RAM Capture and Winpmen - and now [back to DumpIt again](https://www.magnetforensics.com/blog/full-memory-crash-dumps-vs-raw-dumps-which-is-best-for-memory-analysis-for-incident-response)) and then invoke KAPE to handle the triage collection. There were two versions of the code, mostly identical but adapted for use via network share or via USB drive. We used this script within my IR team for a few years before I shared it to GitHub where it's continued to develop (and be the topic of numerous [webcasts](https://bakerstreetforensics.com/presentations/).)

## ARM64 processors

DumpIt, which was recently added to the [Magnet Forensics Free Tools](https://www.magnetforensics.com/free-tools) site, supports x64, x86 and ARM processors on Windows. With CSIRT-Collect and KAPE, I was able to run the triage and RAM collections on my (regular suspects) Windows instances, as well as a Windows ARM virtual machine running on an M1 Mac. That introduced my next problem though, that there wasn't a means within KAPE to detect ARM64 vs x64, just [32bit vs. 64bit](https://ericzimmerman.github.io/KapeDocs/#!Pages%5C2.2-Modules.md). Since the architecture support wasn't there, I whipped up [a new KAPE module](https://github.com/EricZimmerman/KapeFiles/blob/master/Modules/Apps/DumpIt_Memory_ARM.mkape) specific to the ARM version of DumpIt which has been added to the latest version of KAPE. Once you update KAPE, [grab the ARM version of DumpIt](https://www.magnetforensics.com/resources/magnet-dumpit-for-windows/), name it DumpIt_arm.exe, drop it in the /bin and you're good to go. The latest version (4.0) of CSIRT-Collect queries the architecture and will direct KAPE to utilize the appropriate .exe for the architecture.

## Back to batch mode

The first line of the _kape.cli had the (modules) arguments for DumpIt and Encrypted Disk Detector.  
The next line called the KAPE-Triage (targets) collection.
    
    
    --msource C:\ --mdest $dest --module DumpIt_Memory,MagnetForensics_EDD 
    --tsource C:\ --tdest $dest --target KapeTriage --vhdx $env:computername --zv false

By default all the KAPE instances called by the _kape.cli will execute simultaneously. When I was first experimenting with it both the RAM collection and the triage collection would initiate at the same time. While this made for even faster collections on larger targets, there was still the issue of preserving the most volatile data. Back to the documentation, _(it helps to read to the very last line... )_

_"Should you want to limit things to a single KAPE process running at once, adding  `--ul` to one of the entries (it should be the first one in most cases) tells KAPE to wait for each instance of KAPE to exit before starting the next. When using this mode, you will only see one active instance of KAPE vs. multiple instances starting at once."_

Adding a **\--ul** to the first KAPE argument ensured that the memory collection operations would take place first and only then when completed start the next phase of the triage collection.
    
    
    --msource C:\ --mdest $dest --module DumpIt_Memory,MagnetForensics_EDD **--ul**
    --tsource C:\ --tdest $dest --target KapeTriage --vhdx $env:computername --zv false

Another bit about batch mode. When kape.exe executes and there is a _kape.cli in the root path it will use those arguments and then renames the file to $timestamp_kape.cli. So the next time KAPE executes from this directory, a _kape.cli will not be present. Since the intention of the script is to be reusable and repeatable without intrevention, I needed the _kape.cli to be persistent. I decided the solution was to have the script create the _kape.cli at runtime. This also allowed me to accommodate for the x64 vs ARM64 issue. If ARM is detected, the _kape.cli will be generated with the instructions for the ARM module. Otherwise the x64 module will be used.
    
    
    **$arm** = (Get-WmiObject -Class Win32_ComputerSystem).SystemType -match '(ARM)'
    if ($arm -eq "True") {
         Write-Host "ARM detected"
         Set-Content -Path _kape.cli -Value "--msource C:\ --mdest $dest --module **DumpIt_Memory_ARM** ,MagnetForensics_EDD --ul" }
    else {
        Set-Content -Path _kape.cli -Value "--msource C:\ --mdest $dest --module **DumpIt_Memory** ,MagnetForensics_EDD --ul" }
    Add-Content -Path _kape.cli -Value "--tsource C:\ --tdest $dest --target KapeTriage --vhdx $env:computername --zv false"

## Other enhancements

Up until recently the script required specific folder configurations in place (collections folder, memory folder, KAPE, etc.) That's been simplified now. Just sit CSIRT-Collect.ps1 next to your KAPE directory (whether on network or USB) and the script will take care of any folder creation necessary. 

v4.0 Features:

\- "One Script to Rule them All"  
\- Admin permissions check before execution
    
    
    param ([switch]$Elevated)
    function Test-Admin {
        $currentUser = New-Object Security.Principal.WindowsPrincipal $([Security.Principal.WindowsIdentity]::GetCurrent())
        $currentUser.IsInRole([Security.Principal.WindowsBuiltinRole]::Administrator)
    }
    if ((Test-Admin) -eq $false)  {
        if ($elevated) {
        } else {
            Write-host -fore DarkYellow "CSIRT-Collect requires Admin permissions (not detected). Exiting."        
        }
        exit
    }

\- Memory acquisition will use Magnet DumpIt for Windows (previously used Magnet RAM Capture).  
\- Support for x64, ARM64 and x86 architectures.  
\- Both memory acquistion and triage collection now facilitated via KAPE batch mode with _kape.cli dynamically built during execution.  
\- Capture directories now named to $hostname-$timestamp to support multiple collections from the same asset without overwriting.
    
    
    $collection = $env:COMPUTERNAME+$tstamp

\- Alert if Bitlocker key not detected. Both display and (empty) text file updated if encryption key not detected. If key is detected it is written to the output file.
    
    
    (Get-BitLockerVolume -MountPoint C).KeyProtector > $CollectionHostpath\LiveResponse\$collection-key.txt 
    If ($Null -eq (Get-Content "$CollectionHostpath\LiveResponse\$collection-key.txt")) {
    Write-Host -Fore Yellow "Bitlocker key not identified."
    Set-Content -Path $CollectionHostpath\LiveResponse\$collection-key.txt -Value "No Bitlocker key identified for $env:computername"
    }
    Else {
        Write-Host -fore green "Bitlocker key recovered."
    }

\- More efficient use of variables for output files rather than relying on renaming functions during operations  
\- Now just one script for Network or USB usage. Uncomment the "Network Collection" section for network use.
    
    
    ## Network Collection - uncomment the section below for Network use
    **< #
    **Write-Host -Fore Gray "Mapping network drive..."
    $Networkpath = "X:\" 
    If (Test-Path -Path $Networkpath) {
        Write-Host -Fore Gray "Drive Exists already."
    }
    Else {
        # map network drive
        (New-Object -ComObject WScript.Network).MapNetworkDrive("X:","\\Server\Triage")
        # check mapping again
        If (Test-Path -Path $Networkpath) {
            Write-Host -Fore Gray "Drive has been mapped."
        }
        Else {
            Write-Host -Fore Red "Error mapping drive."
        }
    }
    Set-Location X:
    #>
    ## Below is for USB and Network:
    $tstamp = (Get-Date -Format "_yyyyMMddHHmm")
    $collection = $env:COMPUTERNAME+$tstamp
    ...

\- Stopwatch function will calculate the total runtime of the collection.  
\- ASCII art ;) "Ceci n'est pas une pipe."

![](https://bakerstreetforensics.com/wp-content/uploads/2023/01/csirt-collect-arm.png?w=932) ![](https://bakerstreetforensics.com/wp-content/uploads/2023/01/csirt-collect-moriarty.png?w=937) ![](https://bakerstreetforensics.com/wp-content/uploads/2023/01/csirt-collect-moriarty-network.png?w=1024)

I'm very pleased with how this project has continued to develop. Special thanks to Brian Maloney whos question got things started down a new path. If you have other questions or suggestions, feel free to leave them here - or better yet, submit a PR at the [GitHub repo.](https://github.com/dwmetz/CSIRT-Collect/)

On February 21, I'll be discussing CSIRT-Collect **Free Tools to Bolster Your IR Toolkit** at #MVS2023. [Register today](https://magnetvirtualsummit.com/?utm_source=Speaker&utm_medium=Registration&utm_campaign=2023_EV_MVS_Doug_Metz)! 

In April you can catch me in Nashville at the [Magnet User Summit ](https://reg.rainfocus.com/flow/atwmmagnet/mus2023/portal/login)where I'll be discussing [Magnet2Go. Building A ‘Windows To Go’ Drive To Support Offline Collections](https://magnetusersummit.com/sessions/?search=%22Doug%20Metz%22#/) which will also feature CSIRT-Collect.

Can't wait for the conference? Head over to [GitHub](https://github.com/dwmetz/CSIRT-Collect) and grab your copy today.
