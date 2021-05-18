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

## Presenting firewalld

From the [project homepage](https://firewalld.org):

Firewalld provides a dynamically managed firewall with support for network/firewall zones that define the trust level of network connections or interfaces. 

### Installation

Firewalld should be available for most, if not all, Linux distributions.

For Arch:

```sh
sudo pacman -S firewalld
```

For Fedora, CentOS, RHEL:

```sh
sudo dnf install firewalld
```

For openSUSE, SLE:

```sh
sudo zypper in firewalld
```

To enable firewalld, use the command: 

```sh
sudo systemctl enable --now firewalld
```

To check firewalld status, use the command: 

```sh
sudo systemctl status firewalld
```

Firewall rules can be controlled with the `firewall-cmd` command in your terminal. If you want a GUI interface, `firewall-config` is available for your use and should come preinstalled with the firewalld package.

### Firewall zones

Firewalld manages a set of predefined firewall rules with the use of zones. These zones are based on levels of trust a user has on an interface and its connection. 

Different zones will have different rules, i.e. a trusted zone may have more ports open and a less trusted zone will be more restrictive.

#### Levels of trust in zones

Listed below are the default zones available and they are ranked from least trusted to most trusted:

+ **Drop Zone** - all incoming connections are dropped without any warnings.
+ **Block Zone** - similar with drop zone but with ICMP replies enabled.
+ **Public Zone** - interface is connected to an untrusted network but may allow selected connections on case basis. 
+ **External Zone** - this zone is commonly used if your firewall is used as a gateway. This is configured for NAT. 
+ **Internal Zone** - computers are trusted and some extra services are available.
+ **DMZ Zone** - only certain _incoming_ connections are allowed.
+ **Work Zone** - majority of hosts are trusted in the network. More services are allowed here.
+ **Home Zone** - Your home network. Trust all hosts in the network. More services are allowed.
+ **Trusted Zone** - Everyone is trusted. This should be used _very rarely and carefully_.

A zone's rules are automatically applied to an interface which is in a zone. An interface can only be in a single zone.

### Firewalld commands

To check which zone your interface is currently on:

```sh
firewall-cmd --get-default-zone
```

To check which interfaces are active on which zone:

```sh
firewall-cmd --get-active-zone
```

To check what rules are applied on the defaul

```sh
firewall-cmd --list-all
```

To check what zones are available:

```sh
firewall-cmd --get-zones
```

To check what rules are applied to a specific zone (e.g. home):

```sh
firewall-cmd --zone=home --list-all
```
