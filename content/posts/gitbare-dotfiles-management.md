---
title: "Dotfiles Management using a Git Bare Repository"
date: 2021-02-12T14:33:49+08:00
draft: true
author: "Daryl Galvez" 
description: ""
slug: "" 
tags: []
categories: []
externalLink: ""
series: []
---

If you have been using Linux for a while, you may have tried 'distro hopping'. After some time, it begins to be a pain as you try to configure your new distro just the way you like it. If you're like me who has a ton of applications installed, you'll relate to having hours or even days spent crafting your system to your liking.

I have stumbled upon a video by Distrotube showing how he manages his dotfiles using a git bare repository and I have decided to try it our myself.

# Why use git bare?

Here are some advantages in using this technique. 

#### 1. No extra tools required! 

You only need `git` for this to work. Since `git` is pretty much installed by default in most distros, you'd be pretty much set-up already.

#### 2. No need to deal with symlinks. 

One way you can control your dotfiles is if you place them in a folder and send out symlinks to the correct places where your dotfiles are expected to be placed. This is a bit cumbersome especially if you have a lot of configuration files to handle. You can most certainly set up a script to do this for you though.

#### 3. Your files are placed in version control.

You can track changes made in your dotfiles, use different branches if you want to try out a completely different configuration, clone your dotfiles to a new machine or a new installation to replicate what you have in another system, and revert back to a previous configuration. This is all built-in with `git`.

