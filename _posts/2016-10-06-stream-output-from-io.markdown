---
layout: post
title:  Stream output from IO in ruby
date:   2016-10-06
categories: #ruby
---

```ruby
IO.popen('ls -al') do |io|
  while (line = io.gets) do
    puts line
  end
end
```
