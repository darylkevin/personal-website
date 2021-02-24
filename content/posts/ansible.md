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

Ansible is a program that executes "plays" or tasks on a host or several hosts. Tasks can range from a simple shell script execution to complex configuration management. These tasks are declared in a `yml` file where Ansbile can read what you want it to do.

To drive the point more clearly, let me use an example used in a Fedora Magazine article. Basically, when you define a play, it will look like this conceptually:

```sh
for HOST in $HOSTS; do
    ssh $HOST /usr/bin/echo "Hello World"
done
```

The above example is easy to understand but Ansible provides an easier syntax through `yml`. Since the play/s are written in a `yml` file, it can be easily written and understood by everyone, even to those who have not used Ansible before. The same task can be rewritten like so:

```yml
- hosts: all
  tasks:
    - name: Hello World
      ansible.builtin.shell:
      cmd: /usr/bin/echo "Hello World"
```

## Getting started

Ansible is not installed by default in Linux distributions so we need to install it first before we can use it. Refer to Ansible [documentation](https://docs.ansible.com) on how to get it for your specific distro.

You will definitely want to install the **latest** version of Ansible. A lot of new features are introduced to Ansible frequently so by installing the latest version, you will have access to all those good stuff. A number of syntax improvements which makes declaring states easier are introduced in newer versions as well.

My distro of choice is **Fedora** so i'll install Ansible using _dnf_.

```sh
sudo dnf install ansible
```

Once you have ansible installed, you can create a `.yml`

-------------------

You can then run the play using the following command:

```sh
ansible-playbook /path/to/your/play.yml
```

The contents of a play or a playbook basically describes what *state* you want the machine to have. Once you invoke Ansible to execute a play, it will check the host/s against the play and performs *only* the tasks necessary to achieve the state in the workbook. That means, Ansible will skip parts of the play if that state is already present in the current host.

For example, we have defined a play:

```sh
- name: Install the latest version of Apache
  dnf:
    name: httpd
    state: latest
```

If the latest version of Apache (httpd) is found by Ansible to be already installed, it will skip that play entirely.

## Ansible Playbook

Of course, you would not want to invoke Ansible numerous times over different plays just to set up your system; that would be very counter-intuitive. What we can do is group plays into a *playbook*. A playbook is basically a collection of plays. With this you can declare plays in separate `.yml` files and keep the names of those files in the playbook. Once you're done, you can just invoke Ansible in that playbook and it will execute all the plays declared in the playbook `.yml` file.




#### References:
+ [Using Ansible to set up a workstation by Link Dupont](https://fedoramagazine.org/using-ansible-setup-workstation/)

+ [How to manage your workstation configuration with Ansible by Jay LaCroix](https://opensource.com/article/18/3/manage-workstation-ansible)
