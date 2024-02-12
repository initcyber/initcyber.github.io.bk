---
layout: post
title:  "Vulnerability Scanning"
summary: "You need a vuln scanner in your homelab"
author: initcyber
date: '2023-11-18 19:35:23 +0530'
category: ['VulnScanner','Homelab', 'Cybersecurity']
thumbnail: /assets/img/posts/2023-11-18/feat-1.png
keywords: VulnScanner, nessus, homelab
permalink: /blog/NeedVulnScanner/
usemathjax: true
imgdate: 2023-11-18
---

{%- comment -%} Please delete below and place your page content here {%- endcomment -%}
<center><iframe width="560" height="315" src="https://www.youtube.com/embed/olna19zwtoE?si=HjaI18mrkNzWCJDl" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe></center>

Video Here: https://youtu.be/olna19zwtoE?si=K-PJgKt6HW8Q0HJ_

When starting a homelab I’ve seen way too many posts on [Reddit](https://www.reddit.com) and other forums about “how can I host ‘xyz’ website” or “how can I expose ‘abc’ services so my friends can watch my lifes worth of ‘family’ movies”. But people quickly find themselves in trouble because they didn’t take #cybersecurity into account first to ensure their system is up to par to even host these services.

While most will say “use a VPN to a VPS” or “use Cloudflare tunnels” or “only open 80/443” all of which is OK (sorta), you are only as secure as your weakest link. If your web server (be it a VM or dedicated hardware) has unpatched vulnerabilities, you have now potentially exposed those vulnerabilities to the web for everyone to see.

At the bare minimum, companies such as [Tenable](https://www.tenable.com/), [Greenbone (OpenVAS)](https://www.greenbone.net/en/) or [Qualys](https://www.qualys.com/) are offering free vulnerability scanners for your network that you can install and configure. While my career has been been deep into the Nessus/ACAS suite of tools, I’ve recently toyed with Qualys and was quite impressed with their hybrid cloud model of vulnerability scanning (which at the time of this writing offers 3 external ip’s and 16 LAN ip’s to be scanned).

Using a vulnerability scanner provides a ton of insight of vulnerabilities and remediation’s to fix them. Below are some examples from Tenable Essentials (free for up to 16 IP’s).

![Fairly Clean. This is a pfSense firewall on an internal network (not exposed to the outside world) that segregates my lab. DHCP Detection is expected. NTP is something I’m going to look into but I expect it to be a False Positive.](/assets/img/posts/{{page.imgdate}}/2.png){:data-align="center"}

When you click into each vulnerability you get detailed information regarding them, including the fix:

![You can either click on the “see also” link, or look up the solution on your favorite search engine](/assets/img/posts/{{page.imgdate}}/3.png){:data-align="center"}

Just ensure that when you set up your vulnerability scanner, that you are getting good credentialed scans (for Tenable/Nessus/ACAS look at plugin 19506), otherwise you will receive incomplete results. The vulnerability scanners need to have credentialed and elevated privileges in order to understand the contents of each systems patch level, security posture, network settings, etc., (think of what an intruder/hacker would see if they got in) in order to tell you what to remediate/fix.

![Bad Scan Here. Credentialed Checks : no on the bottom line. Plugin 19506 and several other informational plugins tell you a story about your system you are scanning.](/assets/img/posts/{{page.imgdate}}/4.png){:data-align="center"}

Not only will this help you keep a better security posture for your network and internal infrastructure, in the future if you decide to step into the world of Information Technology/Cybersecurity this can be used as experience. In previous interviews, I’ve impressed recruiters/hiring managers by telling them “I run a #homelab with a vulnerability scanner in the middle scanning all of my network devices on a weekly basis”. That opens the door to further conversations where I can deep dive into what I’ve done and my experience in how I use this to ensure my network, devices, and computers are safe.