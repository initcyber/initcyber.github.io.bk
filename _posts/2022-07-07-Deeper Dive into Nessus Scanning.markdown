---
layout: post
title:  "Welcome to devlopr-jekyll !"
summary: "Getting Started using devlopr-jekyll"
author: initcyber
date: '2019-05-22 14:35:23 +0530'
category: jekyll
thumbnail: /assets/img/posts/code.jpg
keywords: devlopr jekyll, how to use devlopr, devlopr, how to use devlopr-jekyll, devlopr-jekyll tutorial,best jekyll themes
permalink: /blog/welcome-to-devlopr-jekyll/
usemathjax: true
imgdate: 2023-11-21
---

{%- comment -%} Please delete below and place your page content here {%- endcomment -%}

So, after a few days of setting up my Nessus (Essentials) Scanner for my network and setting up perfect scenarios for people to look out for I wanted to share some things I’ve seen both in the workplace as well as in my homelab. I have my Nessus Scanner set up to scan most of my important hosts, servers, firewall/router, etc., anything I deem important due to licensing constraints (max of 16 IPs scanned). I wanted to point out some common issues I’ve seen that may be overlooked when people set up their Nessus Scanners and may not know what to look for.

So below is a screenshot of two Linux Servers in my environment that I have scanned, one being a RHEL 8 (splunk.xxxx.xxx) VM and the other being an Ubuntu 22.04 VM (edgevm.xxxx.xxx):

![6 Vulnerabilities in one, none in the other?](/assets/img/posts/{{page.imgdate}}/2.png){:data-align="center"}

From initial look, it would appear that the RHEL VM has 6 Vulnerabilities (true) and the Ubuntu VM has none (Yay!), however it is important to dive deeper into the scans, as any good Cybersecurity Analyst would do. We can see in the following photo a neat little Authentication Failure Plugin - [117885](https://community.tenable.com/s/article/Understanding-Plugin-117885-identifying-Intermittent-Failure-in-Scan-Results) 

![Intermittent Authentication Failure – Plugin # 117885](/assets/img/posts/{{page.imgdate}}/3.png){:data-align="center"}
What does this mean and why did this happen? Let’s click in further:

![Rate Limiting SSH?](/assets/img/posts/{{page.imgdate}}/4.png){:data-align="center"}

Something is Rate Limiting my SSH commands into this VM. This tells me one of two things (from prior experience) – Network Issues (very prevalent when the Nessus Scanner is remote, such as when scanning into AWS Cloud VM’s), Network Issues in very noisy/busy networks (such as major infrastructures), or something on the VM/Host slowing down SSH traffic (this is likely the culprit). I need to check my logs:

![FYI, I set this up as a test for Fail2Ban, but misconfiguration can haunt you and be a learning experience](/assets/img/posts/{{page.imgdate}}/5.png){:data-align="center"}

Fail2Ban (noted here in this fail2ban [blog](https://www.initcyber.com/posts/2022-07-06-Fail%20to%20Ban%20(You%20Need%20It))) was the culprit in blocking my access. Double checking Nessus Plugin [19506](https://www.tenable.com/plugins/nessus/19506) (Nessus Scan Information) tells us that we weren’t getting Credentialed Scans either (another factor that you aren’t getting good scans)

![Credentialed checks : no](/assets/img/posts/{{page.imgdate}}/6.png){:data-align="center"}

After fixing fail2ban (reconfiguring it to not ban my subnet IP’s... for now) I reran the scan again. This time the results look more promising (Still no Vulnerabilities noted – I actually keep this server patched fairly well/up to date.):

![Still no Vulnerabilities noted – I actually keep this server patched fairly well/up to date.](/assets/img/posts/{{page.imgdate}}/7.png){:data-align="center"}

Valid Credentials and No Issues Found as noted above. Plugin 19506 (Nessus Scan Information) also shows a better story:

![Nessus Plugin 19506](/assets/img/posts/{{page.imgdate}}/8.png){:data-align="center"}

There are a number of other Informational Plugins (Info Plugins, I’ve heard some call “Cat 4” or Category 4) plugins as well that are useful to look at and dig through, and some show up depending on what OS/Device you scan. If you want to look at what Software you have on your system? Look for the Software Enumeration Plugin : 22869 (SSH) or 20811 (Windows). The OS Identification? There’s a plugin for that. If Log4j is installed? Yep there’s a plugin. Nessus/Tenable has defined several plugins that you can find [here](https://www.tenable.com/plugins), and if you type in the plugin number or name of what you are looking for, it will give you not only the vulnerability, but also the Risk Information (Risk Factors, CVSS Scores, Severity, etc):


![Nessus Plugin 162735 - Ubuntu Security Update](/assets/img/posts/{{page.imgdate}}/9.png){:data-align="center"}