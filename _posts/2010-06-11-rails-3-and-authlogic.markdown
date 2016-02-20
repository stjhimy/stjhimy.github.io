---
layout: post
title:  "Rails 3 and Authlogic"
date:   2010-06-11
permalink: /posts/06-rails-3-and-authlogic
categories: old
---

tags: Ruby on Rails, Authentication date: 2010-06-11 00:07:15.000000000Z

##The problem

Many users are getting the message when working with the session model:

    undefined method `to_key' for #<UserSession: no credentials provided>

##Getting it done for now

Apparently some group had forked the project and fixed the problem, so if you need to use that for now, the forked project is the best solution.

In your Gemfile just specify the source  of authlogic:

    gem 'authlogic', :git => 'git://github.com/odorcicd/authlogic.git', :branch => 'rails3'

Your Gemfile should looks something like that:

    gem 'rails', '3.0.0.beta4'
    gem 'mysql'
    gem 'authlogic', :git => 'git://github.com/odorcicd/authlogic.git', :branch => 'rails3'

Call the bundle to install the new gem:

    stjhimy@stjhimy-desktop:~/app_test$ bundle install

Run the server:

    stjhimy@stjhimy-desktop:~/app_test$ rails s
    => Booting WEBrick
    => Rails 3.0.0.beta4 application starting in development on http://0.0.0.0:3000
    => Call with -d to detach
    => Ctrl-C to shutdown server
    [2010-06-10 20:56:00] INFO  WEBrick 1.3.1
    [2010-06-10 20:56:00] INFO  ruby 1.8.7 (2010-01-10) [i686-linux]

Go to your login page and everything should be fine by now.

*Ps: Probably this issue will be fixed in a next authlogic version*
