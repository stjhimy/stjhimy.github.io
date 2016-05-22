---
layout: post
title:  Be careful with ruby 'Hash.merge!'
date:   2016-05-22
categories: #ruby
---

{% highlight ruby %}
2.2.4 :001 > def doit(opts)
2.2.4 :002?>     opts[:foobar] = 1
2.2.4 :003?>   end
 => :doit 
2.2.4 :004 > 
2.2.4 :005 >   opts = {}
 => {} 
2.2.4 :006 > doit(opts)
 => 1 
2.2.4 :007 > p opts
{:foobar=>1}
 => {:foobar=>1}
{% endhighlight %}

> Modifying a hash within the function causes a side effect of modifying the caller's hash

extracted from here: [https://github.com/ruby-grape/grape/pull/1386#discussion_r62328712](https://github.com/ruby-grape/grape/pull/1386#discussion_r62328712)
