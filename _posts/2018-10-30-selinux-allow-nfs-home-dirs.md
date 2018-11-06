---
layout: post
title: SELinux Rule for NFS Home Directories
tags: [selinux, linux]
author: Spencer Riner
comment: false
---

```bash
setsebool -P use_nfs_home_dirs 1
```
