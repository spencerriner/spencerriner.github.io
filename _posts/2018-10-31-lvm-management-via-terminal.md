---
layout: post
title: LVM management via terminal
tags: [lvm]
author: Spencer Riner
comment: false
---

# Initial LVM setup

## Create a Physical Volume

```
pvcreate /dev/sda1 /dev/sdb1 /dev/sdc1
```

## Create a Volume Group

```
vgcreate vg_name /dev/sda1 /dev/sdb1 /dev/sdc1
```

## Create a Logical Volume

```
lvcreate -L10G -n lv_name vg_name
```

## Format LV file system

```
# ext4
mkfs.ext4 /dev/vg_name/lv_name

# xfs
mkfs.xfs /dev/vg_name/lv_name
```

# Extend an existing LV

```
# Defined size
lvextend -L100G /dev/vg_name/lv_name

# Add space
lvextend -L+30G /dev/vg_name/lv_name

# Extend to all free space
lvextend -l +100%FREE /dev/vg_name/lv_name
```

## Extend the filesystem

```
# ext4
resize2fs /dev/vg_name/lv_name

# xfs
xfs_growfs /dev/vg_name/lv_name
```
