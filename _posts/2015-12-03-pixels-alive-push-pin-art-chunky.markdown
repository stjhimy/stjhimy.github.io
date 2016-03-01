---
layout: post
title:  Pixels alive, push pin art and ChunkyPNG
date:   2015-12-03
categories: old
---

Me and my girlfriend decided to create a push pin artwork during the end 2014. If you never saw a push pin art, here's an example:

[https://www.youtube.com/watch?v=YOh7fcrVMSE](https://www.youtube.com/watch?v=YOh7fcrVMSE)

Basically you replicate pixels in a board. We decided to use an image combining Freud and Nietzsche. After having the image done we needed to count the pixels of each color and mount the board with the proper matrix.

<a href="http://imgur.com/3OX7rXP"><img src="http://i.imgur.com/3OX7rXP.jpg" title="source: imgur.com" /></a>

## ChunkyPNG to the rescue

You can easily transform the image in a matrix of pixels using ChunkyPNG:

    require "chunky_png"
    require "active_support/all"
    require "pp"

    img = ChunkyPNG::Image.from_file('./image.png')
    pp img.pixels


    4294967295, 4294967295, 4294967295, 4294967295, 4294967295, 4294967295, 4294967295, 4294967295, 4294967295, 4294967295, 4294967295, 4294967295, 4294967295, 4294967295, 4294967295, 4294967295, 4294967295, 4294967295, 4294967295, 4294967295, 4294967295, 4294967295, 4294967295, 4294967295, 4294967295,
    4294967295, 4294967295, 4294967295, 4294967295, 4294967295, 4294967295, 4294967295, 4294967295, 2627575039, 4294967295, 2627575039, 2627575039, 2627575039, 2627575039, 2627575039, 2627575039, 2627575039, 2627575039, 2627575039, 2627575039, 2627575039, 255, 2627575039, 2627575039, 2627575039, 2627575039, 2627575039, 2627575039, 2627575039, 2627575039, 2627575039, 2627575039, 2627575039, 2627575039, 2627575039, 2627575039, 2627575039, 2627575039, 2627575039, 2627575039

Use active_support Array method "in_groups_of" to split the lines:

    pp img.pixels.in_groups_of(65)

## Final result

<blockquote class="imgur-embed-pub" lang="en" data-id="a/FH28N"><a href="//imgur.com/a/FH28N">pixels_alive</a></blockquote><script async src="//s.imgur.com/min/embed.js" charset="utf-8"></script>
