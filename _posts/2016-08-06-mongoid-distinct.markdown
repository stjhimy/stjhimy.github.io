---
layout: post
title:  Mongoid .distinct
date:   2016-08-06
categories: #ruby #mongodb
---


In ruby when you want unique elements you can do:

```ruby
array = [1, 2, 3, 2, 1]
 => [1, 2, 3, 2, 1]

array.uniq
 => [1, 2, 3]
```

When doing the same using any database collection/table:

```ruby
  Person.all.map(&:name).uniq
 => ["James", "Jhon"]
```

This can get really slow depending on the size of the collection.
Gladly mongo has a faster solution:

```ruby
  Person.distinct(:name)
 => ["James", "Jhon"]
```
