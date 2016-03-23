---
layout: post
title:  "Mind the bulk size: Elasticsearch"
date:   2016-03-22
categories: #elasticsearch
---

When having issues with the Elasticsearch indexing processing make sure to check the bulk size.

It's natural to use tools like `elasticsearch-model` for ruby, it has a bulk size default to 1000.

The size matters when your documents starts to get bigger, the elasticsearch docs already prevents that if you are lucky:

> - 1,000 documents at 1 KB each is 1 MB.
> - 1,000 documents at 100 KB each is 100 MB.

> Those are drastically different bulk sizes. Bulks need to be loaded into memory at the coordinating node, so it is the physical size of the bulk that is more important than the document count.
> Start with a bulk size around 5–15 MB and slowly increase it until you do not see performance gains anymore. Then start increasing the concurrency of your bulk ingestion (multiple threads, and so forth).


Decreasing the bulk from `1000` to `350` gave us some much needed speed.


{% highlight ruby %}
  # Ruby
  Model.import bulk_size: 350
{% endhighlight %}
