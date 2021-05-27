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

If in case that `ufw` is not installed, you can install it using your distro's package manager.

More information for UFW can be found in the `man pages`:

```sh
man ufw
```

### UFW commands

True to its namesake, `ufw` commands and syntax are simple and easy to follow. We'll take a look at some of the most commonly used commands that you'll likely use in your system.

#### Checking UFW status

To check the status of your `ufw` (active or not), run:

```sh
sudo ufw status
```

This will show you the `status` of `ufw` as well as the `ports and protocols` under the `To` column, `Allow, Deny, or Drop rules` under the `Action` column, and the `From` column shows you where the firewall rules/traffic will be applied. 

IPv4 and IPv6 rules are both shown.

#### UFW configuration settings

UFW configuration file is located in `/etc/default/ufw` 

#### Disable UFW 

To disable `ufw`:

```sh
sudo ufw disable
```

If you are running `systemd`, you should also check `ufw` status with `systemctl`:

```sh
sudo systemctl status ufw
```
And if the service is still running, you can stop the service by issuing the command below:

```sh
sudo systemctl stop ufw
```

#### Reset UFW rules

If you are unhappy with the default `ufw` rules and want to start from scratch, you can run:

```sh
sudo ufw reset
```

A confirmation prompt will appear and will ask you if you want to proceed. 

#### Setting up default UFW rules

The default settings for `ufw` are: `Deny` for `incoming` and `Allow` for `outgoing`.

To set them up manually:

```sh
sudo ufw default deny incoming
sudo ufw default allow outgoing
```
