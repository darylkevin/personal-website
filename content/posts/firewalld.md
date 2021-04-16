---
title: "Fiddling with firewalld"
date: 2021-04-13T17:53:46+08:00
draft: true
author: "Daryl Galvez" 
description: "Exploring firewalld in depth and applying it to our Linux distro."
slug: "" 
tags: ["security", "firewall", "network"]
categories: []
externalLink: ""
series: []
---

Before delving into information technology and security, I didn't put too much thought into how I use computers. I did not know how to use and configure firewalls yet alone, make my computer secure. 

After taking a completing the CompTIA trifecta, I had a newly found appreciation with network security. I want to share what I learned to you with emphasis on how to apply it to a Linux machine.

## What is a firewall?

According to the definition from [Wikipedia](https://en.wikipedia.org), a firewall is a network security system that monitors and controls incoming and outgoing traffic based on a predetermined security rules.

Simply put in networking terms, a firewall is simply a packet filter. It allows certain packets to traverse your network from authorized or trusted sources and blocks packets from traversing if it comes from unauthorized or unknown sources.

