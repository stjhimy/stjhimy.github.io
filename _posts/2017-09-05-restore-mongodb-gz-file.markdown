---
layout: post
title:  Restore mongodb from compressed gz file
date:   2017-09-04
categories: #mongodb
---

```
  gunzip -c filename.gz | psql -U user --dbname dbname
```
