---
layout: post
title:  Dir.glob case insensitive
date:   2016-09-13
categories: #ruby
---

Let's say you have two files: `1.jpg` and `2.JPG`

```
~/dir ❯❯❯ ls -al
-rw-r--r--  1 stjhimy  staff     0B Sep 13 10:00 1.jpg
-rw-r--r--  1 stjhimy  staff     0B Sep 13 10:00 2.JPG
```

```ruby
Dir.glob('./*.jpg')
 => ["./1.jpg"]
```

`File::FNM_CASEFOLD` makes it case insensitive:

```ruby
Dir.glob('./*.jpg', File::FNM_CASEFOLD)
 => ["./1.jpg", "./2.JPG"]
```
