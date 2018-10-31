---
layout: post
title: Edit and Update Grub/Grub2
tags: [linux, bootloader]
author: Spencer Riner
comment: false
---

# CentOS 7

Edit `/etc/default/grub` to your liking.

Make changes effective with `grub2-mkconfig`.

```
grub2-mkconfig -o /boot/grub2/grub.cfg
```

UEFI systems use a different path.

```
grub2-mkconfig -o /boot/efi/EFI/centos/grub.cfg
```

# CentOS 6

Edit `/boot/grub/grub.conf`.
