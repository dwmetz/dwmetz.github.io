---
layout: post
permalink: http://bakerstreetforensics.com/2022/01/07/quickpcap-capturing-a-pcap-with-powershell/
title: QuickPcap - Capturing a PCAP with PowerShell
description: None
date: 2022-01-07 21:14:17 -0000
last_modified_at: 2023-09-01 02:07:04 -0000
publish: true
pin: false
categories:
- DFIR
- PowerShell
tags:
- .ETL
- PCAP
- Wireshark
---
Earlier today I was asked for a 'quick and easy' PowerShell to grab a packet capture on a Windows box. I didn't have anything on hand so I set off to the Google and returned with the necessary ingredients.

The star of the show is [netsh trace](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj129382\(v=ws.11\)), which is built into Windows. If we wanted to capture for 90 seconds, start the trace, wait 90 seconds, and stop it the syntax would be:
    
    
    netsh trace start capture=yes IPv4.Address=192.168.1.167 tracefile=c:\temp\capture.etl
    Start-Sleep 90
    netsh trace stop

  * Note there are 3 lines (the first may wrap depending on windows size)



Like Wireshark, you need to specify what interface you want to capture traffic from. In the example above 192.168.1.167 is the active interface I want to capture. But what if I want to use this for automation and won't know in advance what the active IP address will be?

We can grab the local IPv4 address and save it as a variable.
    
    
    #Get the local IPv4 address
    $env:HostIP = (
        Get-NetIPConfiguration |
        Where-Object {
            $_.IPv4DefaultGateway -ne $null -and
            $_.NetAdapter.Status -ne "Disconnected"
        }
    ).IPv4Address.IPAddress

Now putting the two together:
    
    
    $env:HostIP = (
        Get-NetIPConfiguration |
        Where-Object {
            $_.IPv4DefaultGateway -ne $null -and
            $_.NetAdapter.Status -ne "Disconnected"
        }
    ).IPv4Address.IPAddress
    netsh trace start capture=yes IPv4.Address=$env:HostIP tracefile=c:\temp\capture.etl
    Start-Sleep 90
    netsh trace stop

Perfect. Automated packet capture without having to install Wireshark on the host. The only item you should need to adjust will be the capture (sleep) timer.

But wait, the request was for a pcap file. Not a .etl. Lucky for us there's an easy conversion utility [etl2pcapng](https://github.com/microsoft/etl2pcapng/releases). Execution is as simple as giving the exe the source and destination files.
    
    
    ./etl2pcapng.exe c:\temp\capture.etl c:\temp\capture.pcap

That's it. We're now able to collect a packet capture on Windows hosts without adding any additional tools. We can then take those collections and convert them with ease to everyone's favorite packet analyzer.

I've combined everything above into [QuickPcap.ps1](https://github.com/dwmetz/QuickPcap) available on my GitHub site.

![](https://bakerstreetforensics.com/wp-content/uploads/2022/01/quickpcap-1.png?w=1024)QuickPcap.ps1

In this case the capture and conversion are running as one contiguous process, but it's easy to imagine them as separate automation elements being handled through scripting by different processes. After all, we all build our Lego's differently, don't we?

![](https://bakerstreetforensics.com/wp-content/uploads/2022/01/sherlock_lego.jpg?w=960)"The Game is On!"

## Post-update

Since this continues to be one of the most searched for topics, be sure to check out **detonaRE** , a malware detonation and capture utility that uses the same pcap functionality. 

https://youtu.be/lHi7zH9BicM?si=q6yzOBVIMgtZduZZ 

**detonaRE** initiates packet capture and process monitor, detonates the malware, ends pcap collection, completes evidence capture with Magnet RESPONSE. PCAP, Zip, and CSV outputs. 

blog: [https://bakerstreetforensics.com/2023...](https://www.youtube.com/redirect?event=video_description&redir_token=QUFFLUhqa3FldDFjXzBnVUJvN2ZoMzRadlNoTjJ4bjZsUXxBQ3Jtc0ttdmxjaXpmZmJXX09WMkF4c0d1OWFaaFRYTG5kbU00eXlSaWNNd1J6Wk91a0UzbFNDaDJsVTN1cGJOUjVfc0ZzLTZneTB0aW0ybDFqQm9PTHFOQzZHUVdybnFPYmFiTkVZQjRkQ2lwZzA0LUdxY1Zycw&q=https%3A%2F%2Fbakerstreetforensics.com%2F2023%2F08%2F10%2Fcapturing-malware-evidence-with-detonare%2F&v=lHi7zH9BicM)

GitHub: [https://github.com/dwmetz/detonaRE](https://www.youtube.com/redirect?event=video_description&redir_token=QUFFLUhqbGdTYVk4MlFta1Nma2xWejlhZjdMdEpHbEw0Z3xBQ3Jtc0tsaGlDU1RLLVFZT3lJOUF3eHRwVHprdTRsc1lIcWNGYkhaWklqQ3RUT21OUzN3eUdPLTE0c0pRaWxiZHU5OHVxN2QyUVpzQlhuVWpRV3Q1ZFlFWmYwcmNlNFpPYnhZOXZKNzJRc1ItbFFqajVqcWpuRQ&q=https%3A%2F%2Fgithub.com%2Fdwmetz%2FdetonaRE&v=lHi7zH9BicM)
