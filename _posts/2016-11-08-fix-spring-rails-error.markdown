---
layout: post
title:  Fix rails + spring error
date:   2016-11-08
categories: #ruby #rails
---

> Warning: You're using Rubygems 2.0.14.1 with Spring. Upgrade to at least Rubygems 2.1.0 and run `gem pristine --all` for better startup performance.

This happens if you are running rvm without a default version, simply fix with:

```
rvm use 2.3.1 --default
```

Or any other version.
