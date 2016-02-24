---
layout: post
title:  "Rack-www gets updated with more options"
date:   2011-05-12
permalink: /posts/21-rack-www-gets-updated-with-more-options
categories: old
---

tags: Rack, Ruby on Rails date: 2011-05-12 21:07:41.000000000Z

##Options for everyone

As I was saying in my [http://github.com](http://github.com) I released the simple gem "rack-www."
Guess I was misunderstood in the last post, so the discussion about the url with or without the www started.

The point I was trying to show is not about if www is better or worse, it was really about to keep your website traffic over a single url with or without www. Got it?
My choice for the www was, as I explained before, because DISQUS was indexing all my comment rows with urls containing the www.

Finally found a way to migrate the comment rows indexed with www to a non www domain, if you are interested check out disqus migrate tool in you admin dashboard, there's a crawler option now to find the new post adress, awesome.

Well, back to the point, after all the discussion I'm releasing a new version of rack-www. Now it allows you to redirect with or without the www and it's also  possible set a message to show while redirecting.

************************************************************

##Usage:

Default

    #redirects all traffic to www
    config.middleware.use Rack::WWW

You can also customize the :www option to true or false:

    #redirects all traffic to the same domain without www
    config.middleware.use Rack::WWW, :www => false

If you like it's also possible to show a message while redirecting the user:

    config.middleware.use Rack::WWW, :www => false, :message => "Redirecting"

You can find the source code and instructions to install at <https://github.com/stjhimy/rack-www>

Hope you enjoy it guys, cya!
