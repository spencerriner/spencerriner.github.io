---
layout: post
title: Show slow startup items
tags: [systemd, centos]
author: Spencer Riner
comment: false
---

Print a list of all running units, ordered by the time they took to initialize.

```bash
systemd-analyze blame
```
