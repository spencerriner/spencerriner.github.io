---
layout: post
title: LVM management via terminal
tags: [lvm]
author: Spencer Riner
comment: false
---

# Initial LVM setup

## Create a Physical Volume

```bash
pvcreate /dev/sda1 /dev/sdb1 /dev/sdc1
```

## Create a Volume Group

```bash
vgcreate vg_name /dev/sda1 /dev/sdb1 /dev/sdc1
```

## Create a Logical Volume

```bash
lvcreate -L10G -n lv_name vg_name
```

## Format LV file system

```bash
# ext4
mkfs.ext4 /dev/vg_name/lv_name

# xfs
mkfs.xfs /dev/vg_name/lv_name
```

# Extend an existing LV

```bash
# Defined size
lvextend -L100G /dev/vg_name/lv_name

# Add space
lvextend -L+30G /dev/vg_name/lv_name

# Extend to all free space
lvextend -l +100%FREE /dev/vg_name/lv_name
```

## Extend the filesystem

```bash
# ext4
resize2fs /dev/vg_name/lv_name

# xfs
xfs_growfs /dev/vg_name/lv_name
```
