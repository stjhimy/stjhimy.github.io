---
layout: post
title:  Copy pgsql table (schema and data)
date:   2016-07-01
categories: #sql
---

{% highlight sql%}
create table table_copy as
  select * from table;
{% endhighlight %}
