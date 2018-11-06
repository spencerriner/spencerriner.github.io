---
layout: post
title: Rebuild initrd
tags: [linux, kernel]
author: Spencer Riner
comment: false
---

Backup current initrd.

```bash
cp /boot/initramfs-$(uname -r).img /boot/initramfs-$(uname -r).img.bak
```

Rebuild the initramfs for current kernel version.

```bash
dracut -f
```

To rebuild a specified kernel version, use `--kver`.

```bash
dracut --kver 3.10.0-862.11.6.el7.x86_64
```
