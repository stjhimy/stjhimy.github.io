---
layout: post
title:  Copy table (schema and data) using psql
date:   2016-07-01
categories: #sql
---

{% highlight sql%}
create table table_copy as
  select * from table;
{% endhighlight %}
