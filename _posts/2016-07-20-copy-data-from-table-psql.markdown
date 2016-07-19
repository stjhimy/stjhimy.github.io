---
layout: post
title:  Copy data from table using psql (AWS Redshift)
date:   2016-07-20
categories: #sql
---

Assuming both tables have the same fields:

{% highlight sql%}
insert into table
  select * from another_table;
{% endhighlight %}
