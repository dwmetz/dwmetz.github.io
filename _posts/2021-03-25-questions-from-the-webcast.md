---
layout: post
permalink: http://bakerstreetforensics.com/2021/03/25/questions-from-the-webcast/
title: Questions from the Webcast
description: None
date: 2021-03-25 16:42:50 -0000
last_modified_at: 2021-03-25 16:42:50 -0000
publish: true
pin: false
categories:
- DFIR
- Forensic Imaging
- PowerShell
tags:
- PowerShell; DFIR; Github; Presentations
---
![](https://bakerstreetforensics.com/wp-content/uploads/2021/03/bd9d30678d84108c4f299e30e886f74a.jpg?w=540)

Recently my session on **PowerShell Tools for IR Forensics Collection** was re-broadcast by Magnet Forensics. During the event there were a few questions and I thought I'd share my responses here.

If you missed the presentation, just look to the previous post and you'll find a link for YouTube.

**_Does the CSIRT script check for sufficient available space for the temp files before running? I've run into this issue with KAPE collections that get a lot of event logs._**

No it doesn't. Depending on the artifact collection type, the output sizes can vary greatly.  Once you have a collection script that you want to use as your default, I’d measure what the average size is. In all my collection processes I like to make sure I have 1.5x available free space for what I anticipate collecting.  A WMI ‘check’ could be built into the script to verify the freespace vs. expected collection needs.

Example WMI command for available freespace:
    
    
    Get-WmiObject -Class Win32_LogicalDisk | ? {$_. DriveType -eq 3} | select DeviceID, {$_.Size /1GB}, {$_.FreeSpace /1GB}

To get the current size of the event logs via WMI:
    
    
    $WMI = Get-CimInstance -ClassName 'Win32_NTEventlogfile'
    $WMI |Format-Table -AutoSize

This will present the available free space on any fixed disks attached to the system.

The best utilization of free space I could come up with was to grab the memory first, compress it, ship it off and then repeat the collection, compression and transfer with KAPE. This minimizes the amount of disk space needed on the remote host. Both processes have a clean-up operation where all local data is deleted from the endpoint once the network transfer has successfully completed.

**_With memory sizes so big lately, is it possible to configure the script to collect the important artifacts from memory, rather than the entire memory (e.g. process listing, network connections, etc.)?_**

It would be possible to generate that information on the endpoint using a series of PowerShell commands and write the output to a text file (Get-Process, Get-NetTCPConnection, etc.).  This is certainly useful from an IR perspective, but the only artifact that would be returned back would be the output file. Depending on the circumstances of the investigation you may still need/want the full memory image as evidence.

**_Do we have the list of artifacts that are being collected here?_**

In the example presented we’re leveraging the SANS Triage KAPE collection target. The specific collection template used by the CSIRT-Collect script can be adjusted by changing the KAPE command options in the script. You can view the details for any KAPE target by either double-clicking the entry in the KAPE gui, or by viewing the corresponding .tkape file in the program directory (use your text editor of choice).  For the **SANS Triage collection** , the following artifacts are gathered:

# Event Logs  
---  
# Evidence of Execution  
        Name: Prefetch  
        Name: RecentFileCache  
        Name: Amcache  
        Name: Amcache transaction files  
        Name: Syscache  
        Name: Syscache transaction files  
        Name: PowerShell Console Log  
# File System      
        Name: $MFT  
        Name: $LogFile  
        Name: $J  
        Name: $Max  
        Name: $SDS  
        Name: $Boot  
        Name: $T  
# LNK Files and JumpLists         
        Name: Lnk files from Recent  
        Comment: Also includes automatic and custom jumplist directories  
        Name: Lnk files from Microsoft Office Recent  
        Name: Lnk files from Recent (XP)  
        Name: Desktop lnk files XP  
        Name: Desktop lnk files  
        Name: Restore point lnk files XP  
# Recycle Bin and Recycler  
        Name: $Recycle.Bin  
        Name: RECYCLER WinXP  
# System Registry Files  
        Name: SAM registry transaction files  
        Name: SECURITY registry transaction files  
        Name: SOFTWARE registry transaction files  
        Name: SYSTEM registry transaction files  
        Name: SAM registry hive  
        Name: SECURITY registry hive  
        Name: SOFTWARE registry hive  
        Name: SYSTEM registry hive  
        Name: RegBack registry transaction files  
        Name: SAM registry hive (RegBack)  
        Name: SECURITY registry hive (RegBack)  
        Name: SOFTWARE registry hive (RegBack)  
        Name: SYSTEM registry hive (RegBack)  
        Name: SYSTEM registry hive (RegBack)  
        Name: System Profile registry hive  
        Name: System Profile registry transaction files  
        Name: Local Service registry hive  
        Name: Local Service registry transaction files  
        Name: Network Service registry hive  
        Name: Network Service registry transaction files  
        Name: System Restore Points Registry Hives (XP)  
# User Registry Files  
        Name: ntuser.dat registry hive XP  
        Name: ntuser.dat registry hive  
        Name: ntuser.dat registry transaction files  
        Name: ntuser.dat DEFAULT registry hive  
        Name: ntuser.dat DEFAULT transaction files  
        Name: UsrClass.dat registry hive  
        Name: UsrClass.dat registry transaction files  
# System Level Artifacts   
# Scheduled Tasks  
        Name: at .job  
        Name: at SchedLgU.txt  
        Name: XML  
        Name: SRUM  
        Name: Thumbcache DB  
# USB Devices Logs  
        Name: Setupapi.log XP  
        Name: Setupapi.log Win7+  
        Name: WindowsIndexSearch  
        Name: WBEM  
# User Communication          
# Outlook PST and OST files  
        Name: PST XP  
        Name: OST XP  
        Name: PST  
        Name: OST  
# Skype  
        Name: main.db (App <v12)  
        Name: skype.db (App +v12)  
        Name: main.db XP  
        Name: main.db Win7+  
        Name: s4l-[username].db (App +v8)  
        Name: leveldb (Skype for Desktop +v8)  
# Web Browser Artifacts         
        Name: Chrome bookmarks XP  
        Name: Chrome Cookies XP  
        Name: Chrome Current Session XP  
        Name: Chrome Current Tabs XP  
        Name: Chrome Favicons XP  
        Name: Chrome History XP  
        Name: Chrome Last Session XP  
        Name: Chrome Last Tabs XP  
        Name: Chrome Preferences XP  
        Name: Chrome Shortcuts XP  
        Name: Chrome Top Sites XP  
        Name: Chrome Visited Links XP  
        Name: Chrome Web Data XP  
        Name: Chrome bookmarks  
        Name: Chrome Cookies  
        Name: Chrome Current Session  
        Name: Chrome Current Tabs  
        Name: Chrome Favicons  
        Name: Chrome History  
        Name: Chrome Last Session  
        Name: Chrome Last Tabs  
       Name: Chrome Preferences  
        Name: Chrome Shortcuts  
        Name: Chrome Top Sites  
        Name: Chrome Visited Links  
        Name: Chrome Web Data  
        Name: Chrome Extension Files  
        Name: Chrome Extension Files XP  
        Name: Edge folder  
        Name: WebcacheV01.dat  
        Name: Firefox Places  
        Name: Firefox Downloads  
        Name: Firefox Form history  
        Name: Firefox Cookies  
        Name: Firefox Signons  
        Name: Firefox Webappstore  
        Name: Firefox Favicons  
        Name: Firefox Addons  
        Name: Firefox Search  
        Name: Firefox Places (XP)  
        Name: Firefox Downloads (XP)     
        Name: Firefox Form history (XP)  
        Name: Firefox Cookies (XP)  
        Name: Firefox Signons (XP)  
        Name: Firefox Webappstore (XP)  
        Name: Firefox Favicons (XP)  
        Name: Firefox Addons (XP)  
        Name: Firefox Search  (XP)  
        Name: Index.dat History  
        Name: Index.dat History subdirectory  
        Name: Index.dat temp internet files  
        Name: Index.dat cookies (XP)  
        Name: Index.dat UserData (XP)  
        Name: Index.dat Office XP  
        Name: Index.dat Office  
        Name: Local Internet Explorer folder  
        Name: Roaming Internet Explorer folder  
        Name: IE 9/10 History  
        Name: IE 9/10 Cache  
        Name: IE 9/10 Cookies  
        Name: IE 9/10 Download History  
        Name: IE 11 Metadata  
        Name: IE 11 Cache  
        Name: IE 11 Cookies  
# Windows Timeline  
        Name: ActivitiesCache.db  
        Name: ActivitiesCache.db-shm  
        Name: ActivitiesCache.db-wal  
  
_Thanks to everyone who participated. If you have further questions feel free to post them here or on the GitHub site_<https://github.com/dwmetz/CSIRT-Collect>
