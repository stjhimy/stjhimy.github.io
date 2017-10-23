---
layout: post
title:  Find trailing spaces in files
date:   2017-10-23
categories: #off-topic
---

```bash
find ./directory -type f -exec egrep -l " +$" {} \;
```
