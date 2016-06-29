---
layout: post
title:  Free up Elasticsearch space
date:   2016-06-29
categories: #elasticsearch
---

Remove all deleted documents from the index:

{% highlight bash%}
/_optimize?only_expunge_deletes=true"
{% endhighlight %}

{% highlight bash%}
curl -XPOST "http://<ADDRESS>/<INDEX>/_optimize?only_expunge_deletes=true"
{% endhighlight %}
