---
layout: post
title:  "Wscrap - Extract machine-readable data"
date:   2015-05-22
categories: old
---

# :wscrap

A weekend project to help extract machine-readable data from websites. I've been using this for a couple of months now, the first version was only a ruby script, last weekend I decide to wrap it in a **very simple** website. 

http://wscrap.stjhimy.com/

## Example

A github blog post:
https://github.com/blog/2004-new-atom-shirt-in-the-shop

Extract it using wscrap:
http://wscrap.stjhimy.com/scrap?url=https%3A%2F%2Fgithub.com%2Fblog%2F2004-new-atom-shirt-in-the-shop

Result:

{% highlight json%}
{
"title": "New Atom Shirt in the Shop Â· GitHub",
"url": "https://github.com/blog/2004-new-atom-shirt-in-the-shop",
"feed": "https://github.com/blog.atom",
"description": null,
"author": "benogle",
"body": "Prepare yourself for the future with the new Atom shirt and Atom Coasters.",
"html_body": "<div class=\"blog-post-body markdown-body\"> <p>Prepare yourself for the future with the new <a href=\"http://github.myshopify.com/products/atom-shirt\">Atom shirt</a> and <a href=\"http://github.myshopify.com/products/atom-coasters\">Atom Coasters</a>.</p>\n\n <p><a href=\"http://github.myshopify.com/products/atom-shirt\"><img src=\"https://cloud.githubusercontent.com/assets/69169/7661190/60a6a044-fb05-11e4-8674-bf31d9f8b66f.jpg\" alt=\"Atom Shirt\"></a></p>\n\n <p><a href=\"http://github.myshopify.com/products/atom-coasters\"><img src=\"https://cloud.githubusercontent.com/assets/69169/7662714/ed9c25a0-fb14-11e4-8a04-4036537be4d7.jpg\" alt=\"Atom Coasters\"></a></p>\n\n  </div>",
"images": [
"https://cloud.githubusercontent.com/assets/69169/7661190/60a6a044-fb05-11e4-8674-bf31d9f8b66f.jpg",
"https://cloud.githubusercontent.com/assets/69169/7662714/ed9c25a0-fb14-11e4-8a04-4036537be4d7.jpg"
],
"links": [
"<a href=\"http://github.myshopify.com/products/atom-shirt\">Atom shirt</a>",
"<a href=\"http://github.myshopify.com/products/atom-coasters\">Atom Coasters</a>",
"<a href=\"http://github.myshopify.com/products/atom-shirt\"><img src=\"https://cloud.githubusercontent.com/assets/69169/7661190/60a6a044-fb05-11e4-8674-bf31d9f8b66f.jpg\" alt=\"Atom Shirt\"></a>",
"<a href=\"http://github.myshopify.com/products/atom-coasters\"><img src=\"https://cloud.githubusercontent.com/assets/69169/7662714/ed9c25a0-fb14-11e4-8a04-4036537be4d7.jpg\" alt=\"Atom Coasters\"></a>"
],
"created_at": "2015-05-22 10:32:03 +0000"
}
{% endhighlight %}

## Building it

The first version was based on the pismo gem, worked well for a few websites but then I decided to write my own version (wrong decision but worked well at the end). Requests are throttled with Rack, cache is done using Redis.

## Limitations

It's a tiny project, runs on a single heroku dyno which gives a few limitations. In order to keep things fast every extracted url is cached on redis for 60 seconds and you have a limit of 30 requests per minute. 
