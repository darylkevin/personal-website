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

To check what rules and services are applied to a specific zone (e.g. home):

```sh
firewall-cmd --zone=home --list-all
```

To check what rules and services are applied to all zones:

```sh
firewall-cmd --list-all-zones
```

To change an interface's zone (e.g. from `public`  to `home`):

```sh
firewall-cmd --zone=home --change-interface=<interface name>
```

Note that this change will not be permanent.

  
To change your default zone (e.g. `public` to `home`):

```sh
firewall-cmd --set-default-zone=<name of zone>
```

To get all the services available:

```sh
firewall-cmd --get-services
```

To get information on all the services listed by the command above, navigate to:

```sh
cd /usr/lib/firewalld/services
```

You can `cat` the file for the description of the services listed. All these services are predefined.

If you want to add a particular service to your zone, you can run the command:

```sh
firewall-cmd --add-service=<name of service>
```

Please note that the service added by the above method will be gone after a reboot/ restart of the service.

If you want a service to be permanently added to a zone, run the command:

```sh
firewall-cmd --permanent --add-service=<name of service>
```

Always add the `--permanent` flag to add changes permanently.

After making changes, reload your firewalld:

```sh
firewall-cmd --reload
```

If you want to list all services activated for a particular zone:

```sh
firewall-cmd --zone=<name of zone> --list-services
```

In case a service is not available on the default list (e.g. a custom service) and you want to add that service's particular port, you can use:

```sh
firewall-cmd --permanent --add-port=<port number>/<protocol>
```

For example:

```sh
firewall-cmd --permanent --add-port=9090/tcp
```

Port ranges can also be defined by the above command:

```sh
firewall-cmd --permanent --add-port=9000-9090/tcp
```

To define a service (i.e. create a custom service) which is not included in one of the defaults, you can copy one of the samples from `/usr/lib/firewalld/services/` into `/etc/firewalld/services/` directory and edit the copied service file to define all that's needed i.e. name, description, ports, and protocols. Note that you can define multiple ports and protocols here. 

```sh
cp /usr/lib/firewalld/services/ssh.xml /etc/firewalld/services/custom_service.xml
vim /etc/firewalld/services/custom_service.xml
```

To add a trusted source (host/s, network) to your firewall rules, use:

```sh
firewall-cmd --permanent --zone=<desired zone> --add-source=<ip address/subnet mask>
```
```sh
firewall-cmd --permanent --zone=public --add-source=192.168.0.222/24
```

All traffic from trusted source is allowed thru the above command.

Don't forget to reload your firewalld.

### Rule Ordering

Firewall rules are applied in a particular order in order to avoid conflict. All zones follow this order from top to bottom:

+ Port forwarding or masquerading rule
+ Logging rules
+ Allow rules
+ Deny rules

If some rules interact/contradict with each other, the first rule that matches gets implemented.

### Rich Rules

Firewalld allows more fine-grained control by the use of rich rules. Rich rules are custom firewall rules.

More details can be found in the `man` pages: `man 5 firewalld.richlanguage`.

```sh
General rule structure

           rule
             [source]
             [destination]
             service|port|protocol|icmp-block|icmp-type|masquerade|forward-port|source-port
             [log]
             [audit]
             [accept|reject|drop|mark]
```

An example of a rich rule being declared is shown below:

```sh
firewall-cmd --permanent --zone=public --add-rich-rules='rule family="ipv4" source address="192.168.0.0/24" service name="tftp" log prefix="tftp" level="info" limit value="1/m" accept'
```

In the above example, you can see just how much fine tuning we can apply to our firewall when we use rich rules. 

## References

+ [RHCE Training - Configuring Firewalld in RHEL 7](https://www.youtube.com/watch?v=jgErVHBz7XI)
