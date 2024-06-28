---
layout: post
title: "AI in the HomeLab"
summary: "AI and Large Language Models isn't just for ChatGPT"
author: initcyber
date: '2024-06-27 18:30:23 +0530'
category: ['AI','OpenAI', 'OpenWebUI']
tags: ai, llm, 
thumbnail: /assets/img/posts/2024-06-27/Thumbnail.png
keywords: ai, homelab, llm, langchain, new
usemathjax: false
permalink: /blog/ai-in-the-homelab/
imgdate: 2024-06-27
---

As technology continues to evolve at a rapid pace, Artificial Intelligence (AI) has become an integral part of our digital lives. In my homelab, I've started delving into the world of Large Language Models, more commonly called AI, ChatGPT, etc., etc. While it's easy to bring up bing.com and immediately have CoPilot readily available (ChatGPT 4 at the time of this writing), or any of the other handfuls of openly available "AI" services available, up to and including various chatbots commonly seen on various shopping sites, having it hosted in your own lab serves many purposes - especially for those wanting to dive deeply into the world of Large Language Models, , Generative AI, AI Agents (which is quickly replacing the commonly seen Chatbots), Langchain, and more. Most importantly, if you have any data you want to ensure stays secure (your PII, banking information, etc), hosting these models at home is imperative.

If you have a computer with a GPU (recommend 8GB vRAM or more), decent amount of RAM (I would recommend 16GB, could get away with less) and a recent processor, you can run your own LLM such as Llama 3 right on your own hardware, its easy with the help of Ollama.


## Self-Hosting AI and LLMs using Ollama

Over the past few years, the ability to self-host Large Language Models (LLMs) using tools like Ollama has made the barriers to entry REALLY low. Windows users now have a binary which can be used and started up without even touching Windows Subsystem for Linux. While this works, I haven't tried it, and still use Windows Subsystem for Linux when I can. Running Ollama allows you to run AI models directly on your own hardware, without relying on cloud-based services. With Ollama, you can download readily available LLM's, and even train and deploy your own LLMs, enabling you to customize their behavior and integrate them with other systems. For simplicity sake, I downloaded Ollama into my Ubuntu installation within WSL, ran it, set it up, downloaded Llama3 (latest as of this blogpost) and ran it as well.

```bash
$ ollama run llama3
```
This downloaded Llama3 automatically (if it wasn't downloaded) and gave me a prompt which I was able to talk to my Large Language Model:

![Cute Llama](/assets/img/posts/{{page.imgdate}}/2.png){:data-align="center"}



## Opening Ports from my computer to my Home Server

So this is great and all, but I wanted more. I wanted a WebUI to talk to my LLM. One problem: My server(s) at home don't host a GPU, only my trusty laptop, and I don't want to necessarily start serving everything from my laptop, only my GPU when needed... (this is very specific to my case). So I did was every other janky homelabber would do that didn't want to spend hundreds of dollars purchasing GPU's for their servers and instead utilize the Nvidia 3070 in their laptop to serve to the rest of the house:

(I opened it up to my home server)

This wasn't that terribly hard, but WSL natively runs behind a NAT, I needed to make WSL act as if it is also part of the network.

Enter: Mirror Mode.

In your User Profile in Windows, create a .wslconfig file and input:
```bash
[wsl2]
networkingMode = mirrored
```

Or let me as Llama3:
![AI doing some work](/assets/img/posts/{{page.imgdate}}/3.png){:data-align="center"}

Now I should be able to see Ollama at my Laptop's Network address (in this case 172.16.100.10) using port 11343

Now I need a Web Interface to ~~abuse~~ use. This sounds like a job for OpenWebUI.


## What is OpenWebUI?


*OpenWebUI is an open-source framework for building web-based interfaces for LLMs and other AI models. By using OpenWebUI, you can create custom interfaces that enable you to interact with your AI models in a user-friendly way. This makes it easy to experiment with different AI applications and integrate them into your daily workflow.*

Ok so that was written by AI. But its basically the point. OpenWebUI is literally the bees knees when it comes to WebUI's for Self Hosted LLM's and AI front ends. Not only can you use it for your own self hosted AI models, you can use it to connect to OpenAI, Grok, Gemini, and other paid and free API endpoints, and their LLM models. While using their models, you won't have the ability to "privatize" your information (I don't care what they say about not using your data, if it doesn't belong within your confines and control/he who controls the data, owns the data - Caveat: Unless there is a MOA/MOU/SLA/NDA/etc in place outlining otherwise)

![OpenWebUI](/assets/img/posts/{{page.imgdate}}/4.png){:data-align="center"}

This thing is freaking cool, and comes with a ton of options for flexibility, plugins, pipelines, and the works. It's constantly updated and actively developed. You can even have it read the responses to you, listen to your voice and record your speech to it (speech to text), create images using Automatic1111, Dall-e, or ComfyUI, upload documents and store them for your LLM's to read and parse through, etc. It's insane.

## Future endeavours for my homelab?

So I'm delving into this, where am I heading from here? I'm starting to really deep-dive LLM's and AI, python programming and Langchain. I'm also going into AI Agents, which is quickly replacing chatbots commonly seen today (Generative AI vs pre-made scripted workflows). 


In my next blog post (Sometime Soon ™), I'll post about exploring AI agents and how I'm using them in my homelab, how these AI agents can automate your day-to-day tasks, making it easier to manage your homelab and freeing up time for more important activities. While I haven't done it yet, some ideas for AI Agents:

- Event Notifications
- Rotation of Encryption Keys
- Slack/Discord Notifications
- Performing Automated Backups, snapshots, deployments, based on usage (I know kubernetes does this, but imagine Generative AI taking over for this and just for funsies using something like Docker instead?)
- Home Automation
- Log Analysis
- Blog Posting












