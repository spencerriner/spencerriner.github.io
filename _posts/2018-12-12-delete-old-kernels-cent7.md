---
layout: post
title: Delete Old Kernels on CentOS 7
tags: [centos]
author: Spencer Riner
comment: false
---

```
yum install yum-utils
package-cleanup --oldkernels --count=2
```
