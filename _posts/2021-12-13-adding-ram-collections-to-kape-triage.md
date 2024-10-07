---
layout: post
permalink: http://bakerstreetforensics.com/2021/12/13/adding-ram-collections-to-kape-triage/
title: Adding RAM collections to KAPE Triage
description: None
date: 2021-12-13 22:07:53 -0000
last_modified_at: 2021-12-13 22:15:26 -0000
publish: true
pin: false
categories:
- DFIR
- PowerShell
- RAM
tags: []
---
If you're utilizing KAPE to collect triage collections, are you also collecting a RAM image with the operating system artifacts?  
Included in the Modules section of KAPE there are three modules that can create a RAM image. The modules for DumpIt and Winpmem have been available for a while. _(I wrote the DumpIt module and Eric Capuano wrote the Winpmem module.)_ Now you also have the option of using **Magnet Ram Capture** as an option. As with any of the KAPE modules if you're calling an executable, you need to ensure the .exe is present in the \modules\bin directory. KAPE does not distribute any 3rd party executables so you need to bring your own. You can download Magnet Ram Capture from the Magnet Free Tools site at [Free Tools - Magnet Forensics](https://www.magnetforensics.com/free-tools/).

_Speaking of Magnet, I should say that I am, as of recent, an employee of Magnet Forensics. All views here, demented and otherwise, are my own and don't reflect the views or opinions of my employer. Now that all the lawyers are smiling, let's continue._

When utilizing KAPE you can either run the Memory collection module by itself...

![](https://bakerstreetforensics.com/wp-content/uploads/2021/12/kape-mrc-selection.png?w=1024)Magnet Ram Capture module in KAPE

  
Or more likely, as part of a triage collection so you can get the necessary artifacts, as well as a RAM dump.

![](https://bakerstreetforensics.com/wp-content/uploads/2021/12/kape-2.png?w=1024)KAPE Triage Collection along with Magnet Ram Capture

  
While this does work to get both the artifacts and RAM capture, there are a couple issues with the process.

  * Best practices for forensics suggest imaging the RAM before making any other changes to the target
  * KAPE executes the Targets first and then proceeds to Modules (RAM collection)
  * After the KAPE memory collection completes the memory image will be included in the specified KAPE output (zip).
  * A copy of the memory image (raw) file stays on the computer, even if KAPE is transferring the data off to a remote location. _This appeared to be the case regardless of which memory capture utility is used._



## CSIRT-Collect v3

CSIRT-Collect is a PowerShell script that I wrote to automate to collection of a RAM image as well as a KAPE triage collection. I wanted to preserve the order of volatility and capture the RAM before any other artifact collection occurs. Version 3 by default leverages Magnet Ram Capture to collect the memory. You can utilize Winpmem or DumpIt with a minor code modification. 

  
**CSIRT-Collect**  
Prerequisites:

Network share location with "Collections" folder. Within 'Collections', 2 subdirectories:

  * Memory, containing Magnet Ram Capture (MRC.exe) and command line version of 7zip (7za.exe)
  * KAPE (default directory as installed)



The script will:

  * map a drive to the "Collections" share, (update the script to reflect the network share for your environment)
  * capture a memory image with Magnet Ram Capture,
  * capture a triage collection with KAPE, 
  * transfer the output back to the network share,
  * create a text flag when the process has completed.

![](https://bakerstreetforensics.com/wp-content/uploads/2021/12/script-start.png?w=1024)Beginning of script. Temp directory on host is empty. Memory and KAPE folder available on Collections share. ![](https://bakerstreetforensics.com/wp-content/uploads/2021/12/capture-ram.png?w=1024)Tools copied from server. RAM collection initiated. ![](https://bakerstreetforensics.com/wp-content/uploads/2021/12/zip-ram.png?w=1024)Once the RAM Capture is completed, the script captures the Windows build info to a text file, and then zips up that with the raw image file to memdump (7zip). ![](https://bakerstreetforensics.com/wp-content/uploads/2021/12/remote-host-receiving-memory.png?w=1024)The memdump zip file containing the RAM and build info is renamed to reflect the hostname of the target. The zip file is transferred to the network into a folder named for the target. ![](https://bakerstreetforensics.com/wp-content/uploads/2021/12/start-kape.png?w=1024)After the zipped memory image (seen here as 18GB compressed to 4.2GB) is transferred to the network, the KAPE triage collection is initiated. Note the C:\Temp\IR directory is gone and collection artifacts saved to C:\Temp\KAPE. ![](https://bakerstreetforensics.com/wp-content/uploads/2021/12/kape-cleanup.png?w=1024)At the end of the KAPE collection, the artifacts are saved to a VHDX file and then compressed as a zip. ![](https://bakerstreetforensics.com/wp-content/uploads/2021/12/script-complete.png?w=1024)Script completes with zipped VHDX and KAPE console logs, transferred to server location. ![](https://bakerstreetforensics.com/wp-content/uploads/2021/12/memory-zip.png?w=970)The zipped memory collection with the windows build text file. ![](https://bakerstreetforensics.com/wp-content/uploads/2021/12/build-info.png?w=745)Windows build info in text file. Use this value to assist in selecting Windows profile for processing with Volatility. (Save the long time required when running kdbgscan).

At the very end of the script a "transfer complete" text file is written to the directory. This is an easy way to know that all the collection activity has completed. If being used for automation, the presence of this file can be used as a trigger to initiate further automation activities.

![](https://bakerstreetforensics.com/wp-content/uploads/2021/12/tx-complete.png?w=577)Contents of transfer-complete.txt.

And that's it. One script to capture RAM, capture a triage image, and then transfer the collections back to the network.

CSIRT-Collect can be initiated on the endpoint manually, invoked by EDR tools, as well as larger collection scenarios where the script can be pushed out via Group Policy.

**Grab your copy of CSIRT-Collect here:**

<https://github.com/dwmetz/CSIRT-Collect>
