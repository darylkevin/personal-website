---
title: "Automate your workstation setup using Ansible"
date: 2021-02-21T16:17:10+08:00
draft: true
author: "Daryl Galvez" 
description: "Use Ansible to set up your workstation"
slug: "" 
tags: [Ansible, Linux, Automation]
categories: []
externalLink: ""
series: []
---
Let's dive into the basics of setting up your workstation environment (whether it is a laptop or a desktop) using a free and open source tool called Ansible.

## What is Ansible?

[Ansible](https://www.ansible.com/) is a free and open-source configuration management and software automation project developed by [Red Hat](https://www.redhat.com/en). It is mostly used in production environments such as in servers for setting them up.

## Why learn Ansible?

Ansible is a powerful tool despite it being easy to use. It can automate most of the tasks that we usually do when we start out with a new system. Most of us apply a few 'tweaks' to our systems to suit our tastes, be it setting up a wallpaper, applying icon themes, or installing and uninstalling programs.

Most of the time, this process is long and repetitive. This is where Ansible comes in.

## ELI5 - Ansible

Ansible is a program that executes "plays" or tasks on a host or several hosts. Tasks can range from a simple shell script execution to complex configuration management. These tasks are declared in a YAML file where Ansbile can read what you want it to do.

To drive the point more clearly, let me use an example used in a Fedora Magazine article. Basically, when you define a play, it will look like this conceptually:

```sh
for HOST in $HOSTS; do
    ssh $HOST /usr/bin/echo "Hello World"
done
```

The above example is easy to understand but Ansible provides an easier syntax through YAML. Since the play/s are written in a YAML file, it can be easily written and understood by everyone, even to those who have not used Ansible before. The same task can be rewritten like so:

```yaml
- hosts: all
  tasks:
    - name: Hello World
      ansible.builtin.shell:
      cmd: /usr/bin/echo "Hello World"
```

We'll get to more examples later.

## Ansible Playbook



#### References:
+ [Using Ansible to set up a workstation by Link Dupont](https://fedoramagazine.org/using-ansible-setup-workstation/)

+ [How to manage your workstation configuration with Ansible by Jay LaCroix](https://opensource.com/article/18/3/manage-workstation-ansible)
