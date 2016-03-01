---
layout: post
title:  Enable nginx gzip on upstream address
date:   2015-08-06
categories: old
---

When enabling gzip on upstream make sure to identify the gzip_types and gzip_proxied:

    location @unicorn {
      gzip on;
      gzip_proxied     any;
      gzip_types       text/css text/plain text/xml application/xml application/javascript application/x-javascript text/javascript application/json text/x-json;
    }

Add "text/x-json" if you want api json responses(grape/sinatra/rails-api) to be gzipped.
