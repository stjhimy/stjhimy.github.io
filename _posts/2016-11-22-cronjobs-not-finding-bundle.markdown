---
layout: post
title:  Cronjobs not finding bundle
date:   2016-11-22
categories: #ruby #bundle #linux
---

In a fresh linux installation you may find this issue when running bundle commands inside a cronjob:

```
/bin/bash: bundle: command not found
```

You can fix it by adding your PATH to the crontab file:

Open crontabs:
```
crontab -e
```

```
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
```
