---
layout: post
title: Set Hostname on OS X
tags: [osx]
author: Spencer Riner
comment: false
---

Set primary hostname (FQDN).

```
sudo scutil --set HostName <new host name>
```

Set Bonjour hostname (local).

```
sudo scutil --set LocalHostName <new host name>
```

Set computer name.

```
sudo scutil --set ComputerName <new name>
```

Flush DNS cache.

```
dscacheutil -flushcache
```

Reboot.
