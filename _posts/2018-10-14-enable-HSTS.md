---
layout: post
title: Enable HSTS
tags: [web, security]
author: Spencer Riner
comment: false
---

# Apache

Set in the `VirtualHost` section of the Apache config file. Could be in multiple locations depending on config and operating system; check `/etc/httpd/conf/httpd.conf`, etc.

```
Header always set Strict-Transport-Security "max-age=63072000; includeSubdomains;"
```

Reload `httpd` or `apache2`.

```
systemctl reload httpd
systemctl reload apache2
```

# Nginx

Set in `/etc/nginx/nginx.conf` or overridden config file. 

```
add_header Strict-Transport-Security "max-age=63072000; includeSubdomains; ";
```

Reload `nginx`. 

```
systemctl reload nginx
```
