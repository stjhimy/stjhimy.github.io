---
layout: post
title:  Sinatra::Reloader .after_reload
date:   2016-07-27
categories: #ruby
---

We added `.after_reload` to `Sinatra::Reloader`!
[https://github.com/sinatra/sinatra/pull/1150](https://github.com/sinatra/sinatra/pull/1150)

Following the ticket on the old `sinatra-contrib` repository
[https://github.com/sinatra/sinatra-contrib/issues/179](https://github.com/sinatra/sinatra-contrib/issues/179)

```ruby
require "sinatra"
require "sinatra/reloader" if development?

also_reload '/path/to/some/file'
dont_reload '/path/to/other/file'
after_reload do
  puts 'reloaded'
end
```
