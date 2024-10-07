---
layout: post
permalink: http://bakerstreetforensics.com/2023/07/27/designing-internet-access-for-compromised-systems/
title: Designing Internet Access for Compromised Systems
description: None
date: 2023-07-27 17:40:33 -0000
last_modified_at: 2023-07-29 02:13:30 -0000
publish: true
pin: false
categories:
- DFIR
- DIY
- USB
tags:
- ESXi
- Forensics
- inetsim
- LTE
- Malware
- REMnux
---
![](https://bakerstreetforensics.com/wp-content/uploads/2023/07/img_8439.jpg?w=1024)

Virtual machines are a godsend when it comes to malware analysis. Granted there a many malware samples that may have capabilities to detect if they are operating in a virtualized environment and thus respond differently. Many, though not all of these, can be mitigated by patching the malware binary, or tricking it into a false result before needing to look at the sample on a bare-metal system. 

When I'm looking at a piece of malware, I'll run it through a number of environments, gradually permitting external access once I have an idea of what the malware's capabilities look like. Initially when detonating samples, I'll have the target endpoint and a [REMnux](https://remnux.org) virtual machine running [inetsim](https://www.inetsim.org/downloads.html) operating on an isolated network. Rather than re-invent the wheel, [here's a solid article](https://petruknisme.medium.com/malware-analysis-lab-in-esxi-isolated-network-c19bdbdc21ba) on setting up an **isolated** network on VMware ESXi. 

At some point I want to enable access to the internet to observe command and control (C2) and any dropper activity. I don't want there to be an avenue for the malware to be able to interact with any other assets whether on my lab network, or outside it. One way to solve this would be networking and introducing a _router_ to broker the network access. It's been a while since I had my CCNA and I had some hesitations about getting it right without impacting other services in a very internet dependent household. What I wound up going with instead is a completely separate internet connection for the _malware network_ utilizing a LTE hot-spot.

I run my lab environment on ESXi environment using an Intel NUC. The model I have only has one onboard NIC. The easiest way to add another physical adapter was with a [USB Gigabit Ethernet adapter](https://amzn.to/3Dz5QS4) for a measly $13 on amazon. ESXi will not detect this adapter out of the box. Follow the process on [this article](https://us.informatiweb-pro.net/virtualization/vmware/vmware-esxi-6-7-use-an-usb-network-adapter.html) to configure the USB network adapter for ESXi. You will need to download the [USB Network Native Driver for ESXi](https://flings.vmware.com/usb-network-native-driver-for-esxi?download_url=https%3A%2F%2Fdownload3.vmware.com%2Fsoftware%2Fvmw-tools%2FUSBNND%2FESXi800-VMKUSB-NIC-FLING-64098182-component-21668107.zip). Be sure to select the appropriate version to match the version of ESXi you're running. _I'm sure there's an interesting story on why VMWare calls these 'Flings' but that knowledge escapes me._

If all goes as it should, and doesn't it always, you should see second physical adapter (vusb0) in the ESXi console.

![](https://bakerstreetforensics.com/wp-content/uploads/2023/07/screenshot-2023-07-27-at-11.04.37e280afam.png?w=1024)USB network adapter shown as **vusb0**

For the secondary internet access, I wound up going with a [Netgear LM1200 LTE Hotspot](https://amzn.to/3OiR1IC). I like this device because you can configure it to use an LTE connection as a backup if your primary wired internet service is down. I may utilize that in the future but for now it's only used on the malware network without any connection to the primary LAN. Based on my current cellular plan I was able to add the minimum hotspot plan for $10/mo. A worthy investment for me for the peace of mind that I'm (less likely) to compromise the rest of my network when experimenting with live malware. It will also (one would hope) keep my home IP off any watchlists for malware beacons, or anyone else tracking where different samples are detonated from. As Mr. Heller sagely said, _"Just because you're paranoid doesn't mean they aren't after you."_

**The same setup could be very useful for responding to compromises in isolated enterprise or manufacturing environments. If you need to have the device access the internet (maybe to upload evidence to you Forensics Service Provider (FSP)), but don't want to maintain a connection to the corporate LAN due to suspected compromise, this solution would work for that.**

Once I had the hotspot up and running, the **LAN** connection on the hotpot gets connected to the USB ethernet adapter. Then go back to ESXi to the isolated network you created before, the one that you were warned "NO UPLINK", and use the 'Add uplink' function and add the vusb0 device. You can adjust the settings on the LTE hotspot for DHCP if needed as long as the device is in Router (not Bridged) mode.

![](https://bakerstreetforensics.com/wp-content/uploads/2023/07/screenshot-2023-07-27-at-11.25.29e280afam.png?w=1024)Malware network with external internet access

That's it. Now when the infected computer needs to get to the internet, all traffic will go through the LTE connection and the infected systems remain isolated from the primary network. 

![](https://bakerstreetforensics.com/wp-content/uploads/2023/07/img_8434.jpg?w=560)Release the hounds and observe

If I'm in a situation where I absolutely need to run the malware on a bare-metal system I can connect using the LTE modem without threat to any of the other physical systems. 
