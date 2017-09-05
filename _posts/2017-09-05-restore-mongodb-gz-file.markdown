---
layout: post
title:  Restore mongodb from compressed gz file
date:   2017-09-04
categories: #elixir
---

```
  gunzip -c filename.gz | psql -U user --dbname dbname
```
