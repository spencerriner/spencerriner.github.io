---
layout: post
title: Disable IPv6 in Manjaro
tags: [manjaro]
author: Spencer Riner
comment: false
---

Add this option to `/etc/default/grub`:

```
GRUB_CMDLINE_LINUX_DEFAULT=" ipv6.disable=1 "
```

Comment out the IPv6 line in `/etc/hosts`:

```
# ::1 localhost ip6-localhost ip6-loopback
```

Generate a new `grub.cfg` file:

```
sudo grub-mkconfig -o /boot/grub/grub.cfg
```

Reboot.
