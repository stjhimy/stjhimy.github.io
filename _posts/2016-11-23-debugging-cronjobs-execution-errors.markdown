---
layout: post
title:  Debugging cronjobs execution errors
date:   2016-11-23
categories: #linux
---

You can output the cronjob to a file and debug it:

```
#0,05,10,20,30,40,45,50,54 * * * * /bin/bash -l -c 'cd /foo/bar && bundle exec rake foo:bar\
	>> /foo/bar/output.log 2>&1'
```

