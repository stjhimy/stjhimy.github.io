---
layout: post
title:  macOS Sierra + openssl + ruby 2.3.1
date:   2016-11-06
categories: #macOS #ruby
---

After updating to macOS Sierra you may get an error about openssl and ruby:

> Could not load OpenSSL.
You must recompile Ruby with OpenSSL support or change the sources in your
Gemfile from 'https' to 'http'. Instructions for compiling with OpenSSL using
RVM are available at rvm.io/packages/openssl.

To fix you need to specify the openssl path when compiling/reinstalling ruby:

```
rvm install 2.3.1 --with-openssl-dir=`brew --prefix openssl`
```

Just in case you are curious, `brew --prefix openssl` returns the path:

 `/usr/local/opt/openssl`
