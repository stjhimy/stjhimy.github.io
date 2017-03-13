---
layout: post
title:  Ruby vs Elixir - json benchmark
date:   2017-03-10
categories: #ruby #elixir
---

A few points before we show the numbers:

 - This is far away from being a scientific experiment
 - I used a MacBook Air (11-inch, Early 2014) 1,7 GHz Intel Core i7 with SSD
 - The json nodes included strings, numbers and dates


## 90k nodes, 50 MB file

**ruby: 5.2 ~ 6.0 seconds**

```ruby
2.3.1 :001 > file = File.read("file.json"); nil
 => nil
2.3.1 :002 > Benchmark.measure{JSON.parse(file).to_json}
 => #<Benchmark::Tms:0x007f9510463f78 @label="", @real=5.641726000001654>
2.3.1 :003 > Benchmark.measure{JSON.parse(file).to_json}
 => #<Benchmark::Tms:0x007f954b7ecdf8 @label="", @real=5.2335819999862>
2.3.1 :004 > Benchmark.measure{JSON.parse(file).to_json}
 => #<Benchmark::Tms:0x007f952c75ff80 @label="", @real=5.775779999996303>
2.3.1 :005 > Benchmark.measure{JSON.parse(file).to_json}
 => #<Benchmark::Tms:0x007f9530863f40 @label="", @real=6.034717000002274>
```

**elixir: 6.3 ~ 6.6 seconds**

```elixir
iex(1)> {:ok, file} = File.read("file.json"); nil
nil
iex(2)> :timer.tc(fn ->  Poison.decode(file) |> Poison.encode; nil end)
{6467583, nil}
iex(3)> :timer.tc(fn ->  Poison.decode(file) |> Poison.encode; nil end)
{6524216, nil}
iex(4)> :timer.tc(fn ->  Poison.decode(file) |> Poison.encode; nil end)
{6636715, nil}
iex(5)> :timer.tc(fn ->  Poison.decode(file) |> Poison.encode; nil end)
{6353399, nil}
```

## 450k nodes, 250 MB file

**ruby: 30 ~ 43 seconds**

```ruby
2.3.1 :001 > file = File.read("file.json"); nil
 => nil
2.3.1 :002 > Benchmark.measure{JSON.parse(file).to_json}
 => #<Benchmark::Tms:0x007ff382896070 @label="", @real=30.37597399999504>
2.3.1 :003 > Benchmark.measure{JSON.parse(file).to_json}
 => #<Benchmark::Tms:0x007ff32e2db648 @label="", @real=43.56279799999902>
2.3.1 :004 > Benchmark.measure{JSON.parse(file).to_json}
 => #<Benchmark::Tms:0x007ff35449bf70 @label="", @real=44.20812600001227>
2.3.1 :005 > Benchmark.measure{JSON.parse(file).to_json}
 => #<Benchmark::Tms:0x007ff311cc3f60 @label="", @real=43.153361000004224>
```

**elixir: 30 ~ 35 seconds**

```elixir
iex(1)> {:ok, file} = File.read("file.json"); nil
nil
iex(2)> :timer.tc(fn ->  Poison.decode(file) |> Poison.encode; nil end)
{30292016, nil}
iex(3)> :timer.tc(fn ->  Poison.decode(file) |> Poison.encode; nil end)
{33436981, nil}
iex(4)> :timer.tc(fn ->  Poison.decode(file) |> Poison.encode; nil end)
{35012181, nil}
iex(5)> :timer.tc(fn ->  Poison.decode(file) |> Poison.encode; nil end)
{35776466, nil}
iex(6)>
```
