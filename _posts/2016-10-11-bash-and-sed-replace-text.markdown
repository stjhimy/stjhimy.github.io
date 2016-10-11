---
layout: post
title:  bash and sed, find and replace
date:   2016-10-11
categories: #bash #linux
---

Working with Elasticsearch can get hard when generating thounsand of files for a bulk import.
What if you need to replace any text on all those files? Fix a typo? Change a value?

Bash and sed makes it quite fast:

```bash
$ ls directory | wc
  31915   31915  957450

$ du -sh directory
37G     gavel
```

```bash
$ sed -i -- 's/"foo":null/"foo":"bar"/g' *
# Search "foo":null and replace with "foo":"bar"
```


