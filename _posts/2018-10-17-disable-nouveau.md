---
layout: post
title: Disable Nouveau on CentOS 7
tags: [centos, nvidia]
author: Spencer Riner
comment: false
---

# Prerequisites

Install latest kernel and download Nvidia driver. Nvidia drivers can be downloaded in `elinks` if graphics aren't available:

```
elinks http://download.nvidia.com/XFree86/Linux-x86_64
```

or using `wget` if the desired version is known:

```
wget http://download.nvidia.com/XFree86/Linux-x86_64/390.87/NVIDIA-Linux-x86_64-390.87.run
```

# Steps

Edit `/etc/default/grub` and add the following line to `GRUB_CMDLINE_LINUX`:

```
rdblacklist=nouveau nouveau.modeset=0
```

Generate new grub config:

```
grub2-mkconfig –o /boot/grub2/grub.cfg
```

Add the following line to `/etc/modprobe.d/blacklist.conf`. Create that file if it doesn't exist.

```
blacklist nouveau
```

Back up old initramfs and generate a new one:

```
mv /boot/initramfs-$(uname -r).img /boot/initramfs-$(uname -r)-nouveau.img
dracut /boot/initramfs-$(uname -r).img $(uname -r)
```

Reboot the computer and install the Nvidia driver.

```
init 3
chmod u+x NVIDIA-Linux-x86_64-390.87.run
./NVIDIA-Linux-x86_64-390.87.run
init 5
```
