---
layout: post
title: Automatic Updates on CentOS/Ubuntu
tags: [security, centos, ubuntu]
author: Spencer Riner
comment: false
---

# CentOS

Install `yum-cron`.

```
yum install yum-cron
```

## 7

Edit `/etc/yum/yum-cron.conf` to enable updates via `apply_updates = yes`.

Enable and start the service.

```
systemctl enable yum-cron
systemctl start yum-cron
```

## 6

Edit `/etc/sysconfig/yum-cron` if needed. Add `YUM_PARAMETER="-x kernel*"` to not automatically install kernel updates.

Enable and start the service.

```
chkconfig yum-cron on
service yum-cron start
```

# Ubuntu

```
sudo apt-get install unattended-upgrades
sudo dpkg-reconfigure unattended-upgrades
```

Can be adjusted via `/etc/apt/apt.conf.d/50unattended-upgrades` if needed.
