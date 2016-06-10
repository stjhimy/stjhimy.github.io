---
layout: post
title:  Updating node.js using 'n'
date:   2016-06-09
categories: #node
---

Quick:

{% highlight bash %}
sudo npm cache clean -f
sudo npm install -g n
sudo n stable
sudo ln -sf /usr/local/n/versions/node/_VERSION_/bin/node /usr/bin/node
{% endhighlight %}
