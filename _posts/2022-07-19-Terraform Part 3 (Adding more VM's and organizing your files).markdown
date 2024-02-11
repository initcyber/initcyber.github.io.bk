---
layout: post
title:  "Welcome to devlopr-jekyll !"
summary: "Getting Started using devlopr-jekyll"
author: initcyber
date: '2019-05-22 14:35:23 +0530'
category: jekyll
thumbnail: /assets/img/posts/2023-05-16/1.jpg
keywords: devlopr jekyll, how to use devlopr, devlopr, how to use devlopr-jekyll, devlopr-jekyll tutorial,best jekyll themes
permalink: /blog/welcome-to-devlopr-jekyll/
usemathjax: true
imgdate: 2023-11-21
---

{%- comment -%} Please delete below and place your page content here {%- endcomment -%}

So, we’ve created our single Ubuntu virtual machine, better yet, we have created 10, 20, 100 Ubuntu Virtual machines, all from one file:

![I didn’t try this but I’m sure it would be fun to watch my server die a slow death.](/assets/img/posts/{{page.imgdate}}/2.png){:data-align="center"}

Now what if we wanted to deploy RedHat virtual machines, Debian Virtual Machines, and already had cloud-init images ready?

There are two choices you could do, you could simply copy all of this:

```bash
resource "proxmox_vm_qemu" "ubuntu-vm" {
    
    #Set this number to how many VM's you need to deploy, comment out if you don't need to deploy more than 1 (adjust "vmid" and "name" as needed)
    count = 1
    # List our target node (this is the node ID of our "cluster")
    # vmid is the virtual machine ID in Proxmox, default starts at 100 and counts up
    # name is the name we will identify our virtual machine as
    # desc is a descriptive name for our virtual machine
    target_node = "pve"
    vmid = "20${count.index + 1}"
    name = "ubuntu-vm-0${count.index + 1}"
    desc = "Testing out virtual machines"

    # Set VM to start on boot (true/false)
    onboot = true 

    # We are cloning this template identified here - This is a variable identified in credentials.auto.tfvars
    clone = var.ubuntu_22_template

    # Set to 1 to enable the QEMU Guest Agent.
    agent = 1
    
    # VM CPU settings - self explanatory
    cores = 2
    sockets = 1
    cpu = "host"    
    
    # VM Memory Settings - Again, self explantory
    memory = 2048

    # VM Network Settings - Same
    network {
        bridge = "vmbr0"
        model  = "virtio"
    }

    # Default to cloud-init
    os_type = "cloud-init"

    # IP Address and Gateway - Again, we are using the count.index variable here, assuming we are NOT going above 10 virtual machines this should be OK.
    ipconfig0 = "ip=10.10.10.2${count.index + 1}/24,gw=10.10.10.1"
    
    # Set user name here
    # ciuser = "your-username"
    # ---
    # Set SSH keys here
    # sshkeys = <<EOF
    # #YOUR-PUBLIC-SSH-KEY
    # EOF
}
```

Change the top line’s “Resource Name” from “ubuntu-vm” to “ubuntu-vm2” (or whatever you want to your liking) and edit the lines of code to match what you need. If we didn’t we would get an error:

![Error](/assets/img/posts/{{page.imgdate}}/feat-1.png){:data-align="center"}

For example, I will use “ubuntu-vm2” as my resource name, change the count to “2” and the IP addresses to 10.10.10.3x, and keep the original as “count = 1” (the lines of code above):

```bash
resource "proxmox_vm_qemu" "ubuntu-vm2" {
    
    #Set this number to how many VM's you need to deploy, comment out if you don't need to deploy more than 1 (adjust "vmid" and "name" as needed)
    count = 2
    # List our target node (this is the node ID of our "cluster")
    # vmid is the virtual machine ID in Proxmox, default starts at 100 and counts up
    # name is the name we will identify our virtual machine as
    # desc is a descriptive name for our virtual machine
    target_node = "pve"
    vmid = "30${count.index + 1}"
    name = "ubuntu-vm-0${count.index + 1}"
    desc = "Testing out virtual machines"

    # Set VM to start on boot (true/false)
    onboot = true 

    # We are cloning this template identified here - This is a variable identified in credentials.auto.tfvars
    clone = var.ubuntu_22_template

    # Set to 1 to enable the QEMU Guest Agent.
    agent = 1
    
    # VM CPU settings - self explanatory
    cores = 2
    sockets = 1
    cpu = "host"    
    
    # VM Memory Settings - Again, self explantory
    memory = 2048

    # VM Network Settings - Same
    network {
        bridge = "vmbr0"
        model  = "virtio"
    }

    # Default to cloud-init
    os_type = "cloud-init"

    # IP Address and Gateway - Again, we are using the count.index variable here, assuming we are NOT going above 10 virtual machines this should be OK.
    ipconfig0 = "ip=10.10.10.3${count.index + 1}/24,gw=10.10.10.1"
    
    # Set user name here
    # ciuser = "your-username"
    # ---
    # Set SSH keys here
    # sshkeys = <<EOF
    # #YOUR-PUBLIC-SSH-KEY
    # EOF
}
```
Now when I run “terraform plan”:

![$ terraform plan](/assets/img/posts/{{page.imgdate}}/3.png){:data-align="center"}

Now we have 3 to add. This is fine and all, but after a while this file can get huge, lines of code can run together, things can get out of hand. My preferred method would be to logically place resources together in the same file as I see fit (for example, maybe all Ubuntu VM’s in one file, RHEL VM’s in another, Debian in another) or sort them by their functions (web application related VM’s in one, docker VM’s in another, etc.). For example:

![VS Code Snippet](/assets/img/posts/{{page.imgdate}}/4.png){:data-align="center"}

Here I have another file called “rhel-clone.tf”. This file is for my RHEL 8 Virtual Machines that I have set up for cloud-init, and within it I have resources set up inside of the file (strikingly similar to the Ubuntu virtual machine template). I can set the count as needed, and when I run it, alongside the ubuntu-clone.tf file it will also be captured as long as I run “terraform apply” within the same folder.