---
layout: post
title:  Nginx proxy_pass caveat
date:   2015-08-05
categories: old
---

<blockquote class="twitter-tweet" lang="en"><p lang="en" dir="ltr">protip: location /foo/ {proxy_pass domain:port/} the / at the end of the domain:port will avoid nginx to send the path /foo/ to the request</p>&mdash; Jhimy F. Villar (@stjhimy) <a href="https://twitter.com/stjhimy/status/577528088243908608">March 16, 2015</a></blockquote>

Explaining this tweet:

Let's say you have /foo proxying to 127.0.0.1:3000

    location /foo/ {
        proxy_pass http://127.0.0.1:3000/;
    }

when you hit /foo it proxy to 127.0.0.1:3000/

    location /foo/ {
        proxy_pass http://127.0.0.1:3000;
    }

when you hit /foo it proxy to 127.0.0.1:3000/foo

The only difference is the "/" at the end of "127.0.1:3000**/**"
