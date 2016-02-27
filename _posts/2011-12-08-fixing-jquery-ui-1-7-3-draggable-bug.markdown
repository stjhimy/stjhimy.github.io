---
layout: post
title:  "Fixing JQuery UI 1.7.3 Draggable bug"
date:   2011-12-08
permalink: /posts/25-fixing-jquery-ui-1-7-3-draggable-bug"
categories: old
---

tags: Javascript, JQuery date: 2011-12-08 19:27:05.000000000Z


Well, I was playing with a project this week and I was kind of stuck on an old JQuery ui version. Turns out dragging images around, using Chrome, was, at the same time, selecting the image, messing all the stuff around. Something like this:

    <script type="text/javascript" language="javascript" charset="utf-8">
      $("img").draggable();
    </script>

******************************************************

![](https://img.skitch.com/20111116-dd3tymfsbgyxyu5n67rtmactr3.jpg)

At the newest version of JQuery ui, the draggable stuff was working normally on Chrome, so I did a research and after an entire day the solution showed up. Fixing it is pretty simple, you should just bind the 'dragstart' on the image and prevendDefault().

    <script type="text/javascript" language="javascript" charset="utf-8">
      $("img").draggable();
      $('img').bind('dragstart', function(event) { event.preventDefault() });
    </script>

![](https://img.skitch.com/20111116-thiajkxg27e4k1nf79wc1qaq3h.jpg)
