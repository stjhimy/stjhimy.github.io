---
layout: post
title:  1 million sinatra routes?
date:   2016-08-13
categories: #ruby
---

Someone asked:

>> Is it possible to create 1M rest endpoints with sinatra?

Straight forward:

```ruby
require 'sinatra'

(1..1000000).each do |n|
  get "/#{n}" do
    "route: / #{n}"
  end
end
```

```bash
ruby test.rb
== Sinatra ...

curl http://localhost:4567/1
route: / 1                                                                                                                          
curl http://localhost:4567/2
route: / 2
```

Issue: Memory usage, time to boot.

## Optimizing it

Someone who intend to use 1 million routes should bare in mind that it can probably be optimized using `Regex` routes.

[http://www.sinatrarb.com/intro.html#Routes](http://www.sinatrarb.com/intro.html#Routes)


```ruby
require 'sinatra'

get /\/([1-1000000])/ do |n|
  n
end
```

- Less memory usage
- Less time to boot
