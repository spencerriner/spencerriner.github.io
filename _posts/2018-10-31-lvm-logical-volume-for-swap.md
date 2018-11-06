---
layout: post
title: Create LVM Logical Volume for Swap
tags: [lvm]
author: Spencer Riner
comment: false
---

Create logical volume.

```bash
lvcreate vg_name -n lv_swap -L 12G
```

Format the swap space.

```bash
mkswap /dev/vg_name/lv_swap
```

Add entry to `/etc/fstab`.

```bash
/dev/vg_name/lv_swap swap swap defaults 0 0
```

Enable the LV.

```bash
swapon -va
```

Test.

```bash
cat /proc/swaps
```
