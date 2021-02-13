---
title: "Dotfiles Management using a Git Bare Repository"
date: 2021-02-12T14:33:49+08:00
draft: false
author: "Daryl Galvez" 
description: "A simple way of managing your dotfiles or configuration files"
slug: "" 
tags: [git, github, dotfiles, configuration files, set up]
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

# How does this work?

This is pretty much how you would use `git`. You can use the same commands for adding files, removing files, staging, committing, and pushing them to a remote repository. 

What's different is that we'll be using a `Git bare repository`. This is basically like the `.git` hidden directory in your normal git repositories. An alias will be used in order for our commands so that it will not interfere with any other local git repositories.

# Getting started

We'll start by initializing a `git bare repository`.

```sh
git init --bare $HOME/.cfg
```

Change `.cfg` to any name you wish. This is just an arbitrary name for our repository. 

Define an alias for the command. You can also use any other alias that you would want. This is for brevity only. We don't want typing lengthy commands everytime!

```sh
alias config='/usr/bin/git --git-dir=$HOME/.cfg/ --work-tree=$HOME'
```

Run this next command to hide all files that we are not explicitly keeping track of.

```sh
config config --local status.showUntrackedFiles no
```

Add the alias to your `.bashrc` or `.zshrc` or `config.fish` for convenience. You can also run this command to save you time.

```sh
echo "alias config='/usr/bin/git --git-dir=$HOME/.cfg/ --work-tree=$HOME'" >> $HOME/.bashrc
```

And that's it! You have successfully set up your `git bare repository`. You can pass `git` commands to your alias and add your dotfiles and other configurations. Of course, this is not limited to just dotfiles. You can add all kinds of files and directories here.

```sh
config status
config add .bashrc
config commit -m "Add bashrc"
config push
```

# Cloning your previous configurations to your new installation

So you have decided to do a clean install of your operating system and you want to have it configured fast. Your `git repository` is here and ready to settle in your new system.

Set up your alias before starting. You may just type it in your terminal or add it to your `.bashrc`, `.zshrc`, `config.fish`.

```sh
alias config='/usr/bin/git --git-dir=$HOME/.cfg/ --work-tree=$HOME'
```

Add your git bare repository to a `.gitignore` file. This will help you avoid any recursion problems.

```sh
echo ".cfg" >> .gitignore
```
Again, `.cfg` is an arbitrary directory name. You can use any folder name you wish. 

You're now ready to clone your dotfiles into a `git bare` repository. Take note that you will need to use your the directory you specified in the previous step, replacing the `.cfg` directory in this command.

```sh
git clone --bare <git-repo-url> $HOME/.cfg
```

Make sure that your alias is defined in your current shell scope. If you have the alias in your `.bashrc`, `.zshrc`, or `config.fish`, you may need to `source` them again for the alias to take effect.

```sh
alias config='/usr/bin/git --git-dir=$HOME/.cfg/ --work-tree=$HOME'
```

Or

```sh
source .bashrc
```

Checkout the contents of your `bare repository` to your `$HOME` directory.

```sh
config checkout
```

Of course, this command might fail as you might have similarly named files already in your new installation. You may see errors such as the one below.

```sh
error: The following untracked working tree files would be overwritten by checkout:
    .bashrc
    .gitignore
Please move or remove them before you can switch branches.
Aborting
```

There is a solution mentioned in an article in Atlassian written by [@durdn](https://www.atlassian.com/git/tutorials/dotfiles). It involes backing up the files using a shell script. I think this is an elegant and efficient way for backing up files causing the checkout error.

```sh
mkdir -p .config-backup && \
config checkout 2>&1 | egrep "\s+\." | awk {'print $1'} | \
xargs -I{} mv {} .config-backup/{}
```

After running the above, run the checkout command again.

```sh
config checkout
```

Personally, I just force the checkout without backing up the files, I usually do this in a new installation so I don't really mind if the old files get overwritten.

```sh
config checkout -f
```

Once you run `config status`, you may see a lot of untracked files again. Let's set the `showUntrackedStatus` flag to `no` again so we'll only see files which we only explicitly track.

`config config --local status.showUntrackedFiles no`

Your set up is now complete and you can now add and update files using your `config` alias and `git` commands.

```sh
config status
config add .bashrc
config commit -m "Add bashrc"
config push
```

This setup may be a bit confusing at first but once you get the hang of it, then you'll see how simple it really is.


Next time, we'll be looking into how to set up your ideal workstation using **Ansible**. 
