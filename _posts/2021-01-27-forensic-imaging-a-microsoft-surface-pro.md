---
layout: post
permalink: http://bakerstreetforensics.com/2021/01/27/forensic-imaging-a-microsoft-surface-pro/
title: Forensic Imaging a Microsoft Surface Pro
description: None
date: 2021-01-27 21:56:55 -0000
last_modified_at: 2021-01-27 21:56:55 -0000
publish: true
pin: false
categories:
- DFIR
- Forensic Imaging
tags:
- Imaging
- Paladin
- Surface Pro
---
### Pre-Requisites:

  * 4-port USB hub
  * Flash Drive ([Paladin](https://sumuri.com/product/paladin-edge-64-bit/) bootable) - <https://sumuri.com/make-your-own-paladin-usb-2/> created with unetbootin -<https://unetbootin.github.io/>
  * USB hard drive for evidence collection, minimum 1.5x capacity of device being imaged
  * Keyboard
  * Mouse
    * *Keyboard/mouse can be either wired USB or one that leverages an RF dongle. (no Bluetooth)



### UEFI Configuration:

Make sure the device is fully powered down (not in standby state) by holding down the power button (15-30 seconds) until the screen goes black.

Remove the Surface Pro keyboard and disconnect any accessories

Boot to the UEFI configuration (BIOS) by holding down the **Volume-Up** button while pressing the **power** button. Release the power button and hold the volume button until you see the Surface logo.

Under **Security**  turn off Secure Boot

![](https://bakerstreetforensics.com/wp-content/uploads/2021/01/img_0949.jpeg?w=520)UEFI Security

Under **Boot configuration  **select “USB Storage” and drag to the top of the list.

![](https://bakerstreetforensics.com/wp-content/uploads/2021/01/img_0950.jpeg?w=520)UEFI Boot configuration

Power off the device again.

### Booting with Paladin

Connect the USB hub to the Surface Pro.

Attached to the USB hub you should have:

  * Flash Drive (Paladin bootable) - https://sumuri.com/make-your-own-paladin-usb-2/
  * USB hard drive for evidence collection
  * Keyboard
  * Mouse
    * *Keyboard/mouse can be either wired USB or one that leverages an RF dongle. (no Bluetooth)

![](https://bakerstreetforensics.com/wp-content/uploads/2021/01/img_3323.jpeg?w=768)USB hub and peripherals

_PRO Tip – if the USB hub has power buttons for the individual devices make sure all the ports are powered on. ;)   Yes, I did spend about 10 minutes troubleshooting this. (Mondays)_

Hold down the **Volume-Down**  key and press the **Power**  button. Continue holding the **Volume-down**  button until you see the Surface logo.

System should now boot to the Paladin USB

![](https://bakerstreetforensics.com/wp-content/uploads/2021/01/img_3324.jpeg?w=1024)Booting from Paladin USB

Select the default (top) option – **Sumiri Paladin Live Session – Forensic Mode**

![](https://bakerstreetforensics.com/wp-content/uploads/2021/01/img_0951.jpeg?w=626)Boot menu selection

Once booting is complete, you will be presented with the Paladin Desktop.

![](https://bakerstreetforensics.com/wp-content/uploads/2021/01/img_3326.jpeg?w=1024)Paladin Desktop on Surface Pro

### Imaging:

Click on shortcut for  **Paladin Toolbox**

Note the Warning about Dates/Times and click OK

![](https://bakerstreetforensics.com/wp-content/uploads/2021/01/img_3327.jpeg?w=1024)Date/time warning

Select the Source Device. In this case I’m choosing **/dev/sda**  which will be the entire disk (3 partitions) on the host hard drive.

Specify the image format: _Expert Witness Format,_  **EWF (E01)**

  
Populate the case details for the EWF based on case requirements

![](https://bakerstreetforensics.com/wp-content/uploads/2021/01/img_3329.jpeg?w=1024)Populate E01 Case Information

Specify the image **Destination**

![](https://bakerstreetforensics.com/wp-content/uploads/2021/01/img_3330.jpeg?w=1024)Specify Destination Drive

Label: $hostname of asset

Check **Verify after creation**

Click**  Start**

![](https://bakerstreetforensics.com/wp-content/uploads/2021/01/img_3331.jpeg?w=1024)Imaging in process

A full disk image and verification will take several hours. When completed you will see Image completed and Verification completed in the green text at the bottom.

Click on the shield in the left corner and select the power button icon to shut down.

Disconnect the bootable USB drive and your destination USB drive.

Verify files/folders created by mounting the external USB drive to your examination system.
