---
title: "Fedora 33 Post Install - Quality of Life tweaks"
date: 2021-04-07T12:26:26+08:00
draft: false
author: "Daryl Galvez" 
description: "Things that I tweak after installing Fedora 33"
slug: "" 
tags: [Linux, Fedora]
categories: []
externalLink: ""
series: []
---

As many of you may know, I am a Linux user and I mostly use Arch, NixOS, or an RPM based distro like Fedora and openSUSE. I usually do some tweaking whenever I have a newly installed system mainly to improve system performance as well as to have my system more visually appealing. 

There are already a handful of post-install tweak guides out there but I will show you what I compiled from them and use personally.

## Speed up DNF

A lot of people will complain that DNF is **slow** and they are correct. Compared to `apt` and `pacman`, `dnf` will feed slow. To counter that problem, we'll add the following lines to our `/etc/dnf/dnf.conf` file.

``` sh
fastestmirror=true
deltarpm=true
max_parallel_downloads=10
```

**Fastest mirror** will allow `dnf` to select the fastest mirror for your downloads. **Delta RPM** will tell `dnf` to download only the delta updates, meaning, it will not download the whole package/update, it will only download the _changes_ from the previous package version. **Max parallel downloads** will enable `dnf` to have up to 10 simultaneous downloads. I am not sure why all of these are not enabled by default.

## Update system

I always do a system update after enabling above in my `/etc/dnf/dnf.conf` file. That way, updates will be faster.

``` sh
sudo dnf upgrade --refresh
sudo dnf check
sudo dnf autoremove
sudo fwupdmgr get-devices
sudo fwupdmgr refresh --force
sudo fwupdmgr get-updates
sudo fwupdmgr update
sudo reboot now
```

## Change hostname

Default hostname is for newly installed fedora release is always `localhost`. I change this to `fedora` using below command:

``` sh
hostnamectl set-hostname fedora
```

## BTRFS file system optimizations

Starting Fedora 33, default filesystem will be btrfs. I have used [Willi Mutschler's guide](https://mutschler.eu) for optimization. Most of what I will write here is a direct excerpt from his blog. 

_**Quote**_ 

- ssd: use SSD specific options for optimal use on SSD and NVME
- noatime: prevent frequent disk writes by instructing the Linux kernel not to store the last access time of files and folders
- space_cache: allows btrfs to store free space cache on the disk to make caching of a block group much quicker
- commit=120: time interval in which data is written to the filesystem (value of 120 is taken from Manjaro’s minimal iso)
- compress=zstd: allows to specify the compression algorithm which we want to use. btrfs provides lzo, zstd and zlib compression algorithms. Based on some Phoronix test cases, zstd seems to be the better performing candidate.
- discard=async: Btrfs Async Discard Support Looks To Be Ready For Linux 5.6

So add these options to your btrfs subvolume mount points in your fstab:

``` sh
sudo nano /etc/fstab
# UUID=47faf958-b80a-43e1-a36f-ca5a932474f7 /                       btrfs   subvol=root,x-systemd.device-timeout=0,ssd,noatime,space_cache,commit=120,compress=zstd,discard=async 0 0
# UUID=04ae92cd-717c-4aaf-bb24-58001be8d334 /boot                   ext4    defaults        1 2
# UUID=C17B-722D                            /boot/efi               vfat    umask=0077,shortname=winnt 0 2
# UUID=47faf958-b80a-43e1-a36f-ca5a932474f7 /home                   btrfs   subvol=home,x-systemd.device-timeout=0,ssd,noatime,space_cache,commit=120,compress=zstd,discard=async 0 0
# UUID=47faf958-b80a-43e1-a36f-ca5a932474f7 /btrfs_pool             btrfs   subvolid=5,x-systemd.device-timeout=0,ssd,noatime,space_cache,commit=120,compress=zstd,discard=async 0 0
sudo mkdir -p /btrfs_pool
sudo mount -a
```
Note that I also add a mountpoint for the btrfs root filesystem (this has always id 5) for easy access of all my subvolumes in /btrfs_pool. You would need to restart to make use of the new options. I usually first run updates and restart prior to restoring my backups, such that my restored files are using the optimized mount options such as compression.

Furthermore, as I am using btrfs discard support, let’s check whether the discard option is passed on in /etc/crypttab (as I am using LUKS to encrypt my drives):

``` sh
sudo nano /etc/crypttab
# luks-fcc669e7-32d5-43b2-ba03-2db6a7f5b33d UUID=fcc669e7-32d5-43b2-ba03-2db6a7f5b33d none discard
```

As both fstrim and discard=async mount option can peacefully co-exist, I also enable fstrim.timer:

```sh 
sudo systemctl enable fstrim.timer
```

_**Unquote**_

## Add ble.sh

I have tried `zsh` and `fish` for my system shells and I found that setting them up can be quite cumbersome. I still prefer to use `bash` due to its speed and since it is already the default shell in most distros anyway. I miss `autocomplete` and `syntax highlighting` feature of both `zsh` and `fish` offers so I searched for a way to have those feature in `bash`. Here is where `ble.sh` will come in.

[Bash Line Editor (ble.sh)](https://github.com/akinomyoga/ble.sh) is a command line editor written in pure Bash scripts which replaces the default GNU Readline. It offers `syntax highlighting`, `enhanced completion`, and `vim editing mode`.

To install, use below commands:

``` sh
# Quick INSTALL to BASHRC (If this doesn't work, please follow Sec 1.3)

git clone --recursive https://github.com/akinomyoga/ble.sh.git
make -C ble.sh install PREFIX=~/.local
echo 'source ~/.local/share/blesh/ble.sh' >> ~/.bashrc
```

## Enable RPM fusion and additional repositories

You need to enable RPM fusion repositories to have access to both **free** and **nonfree** software. 

```sh
sudo dnf install https://mirrors.rpmfusion.org/free/fedora/rpmfusion-free-release-$(rpm -E %fedora).noarch.rpm https://mirrors.rpmfusion.org/nonfree/fedora/rpmfusion-nonfree-release-$(rpm -E %fedora).noarch.rpm
```

### AppStream metadata

RPM Fusion repositories also provide Appstream metadata to enable users to install packages using Gnome Software/KDE Discover.

```sh
sudo dnf groupupdate core
```

### Multimedia post-install

```sh
sudo dnf groupupdate multimedia --setop="install_weak_deps=False" --exclude=PackageKit-gstreamer-plugin
```

## Enable Flathub repository

```sh
flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo
```
  
  
  
That's about it. Other post setup installation are handled by my Ansible playbook. I just pull down my playbook and run ansible and all the other packages should be installed.

Hopefully this has been a help to you in configuring your own setup.
