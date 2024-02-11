---
layout: post
title:  "PFSense and PiHole on ESXI"
summary: "Getting all three of these to play nicely"
author: initcyber
date: '2023-05-02 14:35:23 +0530'
category: PFSense
thumbnail: /assets/img/posts/2023-05-02/1.png
keywords: pfsense, pihole, esxi
permalink: /blog/pfsense-pihole-esxi/
usemathjax: true
imgdate: 2023-05-02
---

{%- comment -%} Please delete below and place your page content here {%- endcomment -%}

### Preface:
For those who utilize ESXI/vSphere and want to install pfSense and PiHole, it's not nearly as straight forward as the Proxmox install, however it may make more sense and be better for your environment depending on what is set up currently in your lab.

![Tailscale Icon - Courtesy of Tailscale.com](/assets/img/posts/{{page.imgdate}}/1.png){:data-align="center"}


### ESXI Install
Install ESXI as normal, starting in ESXI version 7 you can denote how big of an install disk you want to set. I typically always set the bare minimum using

```bash
systemMediaSize=small
```

This can be done by pressing "shift + o" whenever you first fire up the installer.

After you install ESXI, go through the normal log in (on the DCUI) and set your static IP address for your ESXI node, then log in.

### Installing pfSense

Next is pfSense installation is also fairly straightforward. First lets set up our networking:

Under Networking -> Virtual Switches, add two switches, one for LAN and one for WAN. Ensure these are separate from your Management port (just like Proxmox, you will require 3 ethernet connections to your box). You may run into issues with only 2 or one without VLANS already set and/or understanding how to set up Router on a Stick.

![Two Network Switches - LAN and WAN](/assets/img/posts/{{page.imgdate}}/2.png){:data-align="center"}

Then set up Two Port Groups attached to each of the Network Switches you just created (1 for 1 - LAN Network to LAN Switch, WAN Network to WAN Switch).

NOTE: If you are using VLANS and are trunking your VLANS down one port, set VLAN ID to 4095 on the LAN network:

![Two Network Port Groups - LAN and WAN w/ 4095 VLAN](/assets/img/posts/{{page.imgdate}}/3.png){:data-align="center"}

Now install pfSense, For SCSI Controller select LSI Logic Parallel for the hard drive(instead of default VMWare or else you won't be able to install pfSense), select FreeBSD 12, and select your two new network port groups.

Install pfSense as normal.


### PiHole

For PiHole, unlike Proxmox, we will have to install this on a separate VM and not a container. This is REALLY straight forward. Install PiHole, set up DNS, block lists, and point pfSense to it. Nothing to it. There are several guides on the internet on how to set up PiHole using various one liners. 

[Pi Hole installation - Basic Install](https://docs.pi-hole.net/main/basic-install/)

