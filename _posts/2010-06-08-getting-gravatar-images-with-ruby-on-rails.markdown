---
layout: post
title:  "Getting gravatar images with Ruby on Rails"
date:   2010-06-08
permalink: /posts/05-getting-gravatar-images-with-ruby-on-rails
categories: old
---

tags: Ruby, Tips and Tricks date: 2010-06-08 23:20:33.000000000Z


##What is Gravatar?

[A Globally Recognized Avatar](http://en.gravatar.com/)

Your Gravatar is an image that follows you from site to site appearing beside your name when you do things like commenting or posting on a blog. Avatars help to identify your posts on blogs and web forums, so why not on any site?

##Understanding how to get images from Gravatar

When you sign up in gravatar, you need to register an unique e-mail, right?
Gravatar uses that email address to generate an unique MD5.hexdigest key, and then, it links that key to your image file.

Let's take a look at my gravatar image url:

<http://www.gravatar.com/avatar.php?gravatar_id=fc929d292323e019d5c4a353f92d5587>


See? that "id" parameter is the MD5.hexdigest key generated with your email, let's try generate that in our application and check if it's the same:

    Digest::MD5.hexdigest('stjhimy@gmail.com').to_s

** Result => fc929d292323e019d5c4a353f92d5587

Yeah it's the same code, so if you prefer, you can write your own tool to get the gravatar user image: just use that key that was generated and make a string with the entire URL.

##Installing and using gravtastic

Install it like a gem:

    gem install gravtastic

Add to your environment.rb file:

    config.gem 'gravtastic', :version => '>= 2.1.0'

In your model specify that:

    class User < ActiveRecord::Base
       is_gravtastic!
    end

Now in your view you can use that:

    <%= image_tag @user.gravatar_url %>

You can also specify some parameters:

    is_gravtastic :author_email,
      :secure => true,
      :filetype => :gif,
      :size => 120

The entire documentation here <http://github.com/chrislloyd/gravtastic>
That's all, now you can easily get avatars from Gravatar.

*Ps: if you try to get your avatar on your own without gravtastic, remember to require "digest/md5" in your project.*
