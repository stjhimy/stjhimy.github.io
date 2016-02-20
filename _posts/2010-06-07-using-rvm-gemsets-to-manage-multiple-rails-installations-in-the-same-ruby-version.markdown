---
layout: post
title:  "Using RVM GEMSETS to manage multiple rails installations"
date:   2010-06-07
permalink: /posts/04-using-rvm-gemsets-to-manage-multiple-rails-installations-in-the-same-ruby-version
categories: old
---

tags: Ruby, Ruby on Rails date: 2010-06-07 20:32:45.000000000Z

##What about rvm?

RVM (Ruby Version Manager) is a command line tool which allow us to easily install, manage and work with multiple Ruby environments from interpreters to sets of gems.

##The problem with a single Ruby version and multiple Rails versions

Usually new users start playing with RVM when becomes a necessity to test a new version of Ruby and/or Rails.
Basically the default RVM environment for almost every developer is:

Ruby versions:

 - Ruby 1.8.7 => Rails 2.3.5 or later
 - Ruby-Head(1.9.2 or later) => Rails 3.0 pre

Now imagine you have some applications in Rails 2.3.5 and starting a new app in Rails 2.3.8 both on Ruby 1.8.7. If you have already Rails 2.3.5 and you install the new 2.3.8 you will lost the previous version. What we can do now? Lucky RVM is a great tool and the development team already thought about that so we can use GEMSETS to create our custom development environment.

##Creating a new GEMSET with RVM

First we check our installed rubies:

    stjhimy@stjhimy-desktop:~$ rvm list

    rvm Rubies

    => ruby-1.8.7-p249 [ i386 ]
       ruby-head [ i386 ]

    Default Ruby (for new shells)

       ruby-1.8.7-p249 [ i386 ]

    System Ruby

       system [ i386 ]

I'm running ruby 1.8.7, and my default gemset is "global", let's check:

    stjhimy@stjhimy-desktop:~$ rvm gemset list
    gemsets : for ruby-1.8.7-p249 (found in /home/stjhimy/.rvm/gems/)
    global

And now we can create a new gemset:

    stjhimy@stjhimy-desktop:~$ rvm gemset create latest_rails_stable
    Gemset 'latest_rails_stable' created.

If you check your gemsets now:

    stjhimy@stjhimy-desktop:~$ rvm gemset list
    gemsets : for ruby-1.8.7-p249 (found in /home/stjhimy/.rvm/gems/)
    global
    latest_rails_stable

To set RVM to use the new gemset you just need to especify the "rubyverion@gemset", just like that:

    stjhimy@stjhimy-desktop:~$ rvm use 1.8.7@latest_rails_stable
    Using ruby 1.8.7 p249 with gemset latest_rails_stable

Checking now, you don't have any Rails installed in this gemset. To install it just type:

    stjhimy@stjhimy-desktop:~$ rails -v
    The program 'rails' is currently not installed.  You can install it by typing:
    sudo apt-get install rails
    stjhimy@stjhimy-desktop:~$ gem install rails
    Successfully installed activesupport-2.3.8
    Successfully installed activerecord-2.3.8
    Successfully installed rack-1.1.0
    Successfully installed actionpack-2.3.8
    Successfully installed actionmailer-2.3.8
    Successfully installed activeresource-2.3.8
    Successfully installed rails-2.3.8
    7 gems installed
    Installing ri documentation for activesupport-2.3.8...
    Installing ri documentation for activerecord-2.3.8...
    Installing ri documentation for rack-1.1.0...
    Installing ri documentation for actionpack-2.3.8...
    Installing ri documentation for actionmailer-2.3.8...
    Installing ri documentation for activeresource-2.3.8...
    Installing ri documentation for rails-2.3.8...
    Installing RDoc documentation for activesupport-2.3.8...
    Installing RDoc documentation for activerecord-2.3.8...

And that's all, you should be able to switch between your Rails versions in the same Ruby version now using RVM.
