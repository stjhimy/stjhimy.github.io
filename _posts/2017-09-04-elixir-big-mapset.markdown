---
layout: post
title:  Elixir Mapset
date:   2017-09-04
categories: #elixir
---

https://hexdocs.pm/elixir/MapSet.html

Calculating the difference between lists:

```elixir
iex(1)> map1 = [1,2,3,4,5]
[1, 2, 3, 4, 5]
iex(2)> map2 = [1,2,3,6]
[1, 2, 3, 6]
iex(3)> map1 -- map2
[4, 5]
```

For bigger lists/sets (millions of items) use Mapset to make it faster:

```elixir
  Mapset.difference(Mapset.new(map1), Mapset.new(map2))
```
