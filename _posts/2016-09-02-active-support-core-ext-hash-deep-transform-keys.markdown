---
layout: post
title:  active_support deep_transform_keys
date:   2016-09-02
categories: #ruby #rails
---

Even when your project is not built using Rails never forget about `active_support/core_ext`.

Finding/replacing values in a hash can be cumbersome:

```ruby
 > hash = {"foo" => "foo", "bar" => "bar"}
 => {"foo"=>"foo", "bar"=>"bar"}
 > hash.map{|k,v| {k.upcase => v}}.reduce(&:merge)
 => {"FOO"=>"foo", "BAR"=>"bar"}
```

It get's even worse when you have a multi-level hash:

```ruby
 > hash = {"foo" => "foo", "bar" => {"foo" => "foo"}}
 => {"foo"=>"foo", "bar"=>{"foo"=>"foo"}}
 > hash.map{|k,v| {k.upcase => v}}.reduce(&:merge)
 => {"FOO"=>"foo", "BAR"=>{"foo"=>"foo"}}
```

Note that `"BAR"=>{"foo"=>"foo"}` still using `foo` instead of `FOO`. This is caused by the multi-level hash,
to iterate all the levels you need some recursive code, here's when `active_support/core_ext` shines again:

```ruby
 > require 'active_support/core_ext/hash'
 => true
 > hash = {"foo" => "foo", "bar" => {"foo" => "foo"}}
 => {"foo"=>"foo", "bar"=>{"foo"=>"foo"}}
 > hash.deep_transform_keys {|k| k.upcase}
 => {"FOO"=>"foo", "BAR"=>{"FOO"=>"foo"}}
```

`"FOO_BAR"=>{"foo"=>"foo"}`

[http://api.rubyonrails.org/classes/Hash.html#method-i-deep_transform_keys](http://api.rubyonrails.org/classes/Hash.html#method-i-deep_transform_keys)
