---
layout: post
title:  rack-www 2.3 released
date:   2017-07-24
categories: #ruby
---

The gem `rack-www` had been updated to 2.3, this version introduces `host-regex` option:


```ruby
  config.middleware.use Rack::WWW, :host_regex => /example/i
```

> This will only redirect when the host matches example, such as example.com or example1.com. It won't redirect on localhost or any other host that does not match the regex

[https://github.com/stjhimy/rack-www](https://github.com/stjhimy/rack-www)

[https://rubygems.org/gems/rack-www](https://rubygems.org/gems/rack-www)
