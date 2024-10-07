---
layout: post
permalink: http://bakerstreetforensics.com/2023/06/12/raspberry-pi-internet-speed-monitor/
title: Raspberry Pi Internet Speed Monitor
description: None
date: 2023-06-12 20:20:39 -0000
last_modified_at: 2023-06-13 10:38:14 -0000
publish: true
pin: false
image:
  path: https://bakerstreetforensics.com/wp-content/uploads/2023/06/img_8272.jpeg
categories:
- DIY
- Raspberry Pi
- Steampunk
tags:
- CLI
- Grafana
- InfluxDB
- Python
- Speedtest
---
I was looking wistfully at the Lack Rack from my arm chair, admiring the (faux) copper conduit that covered the primary inbound internet link to the switch. I thought it would be cool looking to have an antique steam gauge attached to the piping. Two things caused that idea to quickly change - 1. the going prices for antique steam gauges right now, 2. once I was thinking about it as a gauge I thought an 'internet speed gauge' would be perfect. Alas, even if said gauge could be acquired without breaking the bank, converting MBPS to PSI and making it functional is above my level of engineering. So on to the next best thing - a Raspberry Pi hack.

## **Materials** :

  * Raspberry Pi (3 or 4) with Raspbian 32-bit OS
  * Case with 3.5 in LCD Display
  * Copper spray paint ;)
  * Attention to detail at the command line



## Speedtest CLI

Once you've got your Raspberry Pi up and running start with the **Installing the Speedtest CLI** instructions at <https://pimylifeup.com/raspberry-pi-internet-speed-monitor/>. Complete steps **1-6.** When the article gets to **Writing our Speed Test Python Script** , you can skip that section._I do recommend it from a learning perspective, but the code from that step won't be used in the final project_.

Assuming this is a new installation, you will need to install **InfluxDB** and **Grafana**. Complete the respective instructions for each.

  * Installing InfluxDB: <https://pimylifeup.com/raspberry-pi-influxdb/>
  * Setting up Grafana: <https://pimylifeup.com/raspberry-pi-grafana/>



Continue with the primary article's instructions for **Using Grafana to Display your Speedtest Data**.

If you've made it along this far, you should have a working Grafana dashboard displaying Upload Speed, Download Speed, and Ping (Latency). If you're hitting a glitch - go back through what you've coded and double check that any references to the user (default = Pi) are accurate for the user on your device. You should be seeing updated data based on the frequency you specified in **crontab -e**. 

![](https://bakerstreetforensics.com/wp-content/uploads/2023/06/screenshot-2023-06-12-at-2.57.16e280afpm.png?w=1024)

## Install Grafana Kiosk

Next, we want to set up our device as a kiosk, and have it boot and display the Network Speed dashboard automatically.

Install Grafana Kiosk from <https://github.com/grafana/grafana-kiosk>. For my installation I used the **_ARM v6 grafana-kiosk.linux.armv6_** release. 

## Running the Dashboard on startup:

We're going to use a yaml file to store our dashboard configuration:

Create a new file, **config.yaml** and populate it as such:
    
    
    general:
      kiosk-mode: full
      autofit: true
      lxde: true
      lxde-home: /home/(user)
    target:
      login-method: local
      username: admin
      password: (password)
      playlist: false
      URL: http://localhost:3000/d/bdf20d32-c4ff-4578-a3f4-7a38e1f722b9/network-speed?orgId=1
      ignore-certificate-errors: false

**Be sure to substitute the proper ID wherever you see (user).** The **URL** for the dashboard can be copied from the web interface of the dashboard. 

Edit**/home/(user)/.config/lxsession/LXDE-pi/autostart**

Add a line: _(one line, may show as wrapped)_
    
    
    @/usr/bin/grafana-kiosk -lxde-home /home/(user) -c /home/(user)/config.yaml

Save & Exit.

Now when you reboot the Pi, the dashboard should come up full screen after login. 

![](https://bakerstreetforensics.com/wp-content/uploads/2023/06/img_8273.jpeg?w=768) ![](https://bakerstreetforensics.com/wp-content/uploads/2023/06/img_8272.jpeg?w=768) ![](https://bakerstreetforensics.com/wp-content/uploads/2023/06/img_8271.jpeg?w=768)
