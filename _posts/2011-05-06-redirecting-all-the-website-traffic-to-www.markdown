---
layout: post
title:  "Redirecting all the website traffic to www"
date:   2011-05-06
permalink: /posts/20-redirecting-all-the-website-traffic-to-www
categories: old
---

tags: Rack, Ruby on Rails date: 2011-05-06 21:33:24.000000000Z

##Oh DISQUS...

Last year i was facing some troubles with [Disqus](http://disqus.com/), looks like it was indexing different urls for "http://www.stjhimy.com" and "http://stjhimy.com".
The same page, two different commentary rows, the easier solution was redirect all traffic to www.

##Options

I found 3 ways to do that:

 - Rewrite rule in your nginx/apache server.
 - Before filter in the application_controller.
 - Use rack to filter the requests and redirect all to www.

************************************************************

##Rewrite rule nginx/apache server

It's a good option if you have access to the real server.

##For nginx

In your nginx.conf insert this lines:
    server {
          listen 80;
          server_name example.com;
          rewrite ^/(.*) http://www.example.com/$1 permanent;
        }

##For apache

In your .htaccess insert this lines:
PS: mod_rewrite must be enabled

    Options +FollowSymLinks
    RewriteEngine on
    RewriteCond %{HTTP_HOST} ^example\.com
    RewriteRule ^(.*)$ http://www.example.com/$1 [R=permanent,L]

##Before filter in the application_controller

Pretty simple code, you just need to verify if the current request have or not www at the beginning:

    before_filter do
      unless request.host =~ /^(www.)/
       redirect_to "#{request.protocol + 'www.' + request.host + request.path}"
      end
    end

If the request.host do not start with www. it redirects to the right url.

##Using rack to filter the requests and redirect all to www

All the both options above works fine, but i was looking for a reason to play with rack and all that stuff. So i decide to write the [rack-www](https://github.com/stjhimy/rack-www) gem and get along with rack. It's pretty simple, the gem creates a rack middleware called WWW, it filters the request and verifies if it contains or not the www. Well that's it, if you need something like that one day, those are great options.

##Edited:

Talking to Adam Howard and Ryan Christensen i started thinking that maybe it's interesting to add some options to the gem so the user can chose to add or remove the www.
