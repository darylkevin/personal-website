---
title: "Don't make your firewall complicated"
date: 2021-05-27T09:40:59+08:00
draft: true
author: "Daryl Galvez" 
description: "This will be a tutorial on how to use UFW, another Linux firewall utility"
slug: "" 
tags: [security, firewall, linux]
categories: []
externalLink: ""
series: []
---

## UFW - The Uncomplicated Firewall

In my previous post, we have taken a look at firewalld which is a firewall utility for Linux. For some, firewalld may seem complicated and difficult to use. 
Fortunately, there is another firewall utility which is a bit more user friendly. We'll take a look at UFW or the uncomplicated firewall in this post and see
how this will 'uncomplicate' (pun intended) setting up firewalls in our Linux box.

### What is UFW?

UFW is a utility made to simplify the setup of `netfilter` firewall rules and is designed to be easy to use. It uses a `command-line interface` and uses `iptables` for configuration. 

UFW is installed by default in Debian/Ubuntu based distributions.
