---
layout: post
title:  "Five quick steps to set up RVM with rails 2 and rails3"
date:   2010-08-06
permalink: /posts/10-five-quick-steps-to-set-up-rvm-with-rails-2-and-rails3
categories: old
---

tags: Ruby, Ruby on Rails date: 2010-08-06 00:53:08.000000000Z

##1 - Installing RVM

    $ sudo gem install rvm
and
    $ rvm-install

Finally put this lines in your .bash_profile or .bashrc:

    if [[ -s $HOME/.rvm/scripts/rvm ]] ; then
            source $HOME/.rvm/scripts/rvm
    fi

Restart your terminal, Rvm should be working by now.

##2 - Installing ruby inside your rvm

    $ rvm install 1.8.7

Will install ruby 1.8.7

    $ rvm install ruby-head

Will install ruby 1.9.2 or newer.


##3 - Setting up rvm to use your specific ruby version

    $ rvm use 1.8.7

To use just this time.

    $ rvm use 1.8.7 --default

To use always this version.

If you are using any other ruby replace the 1.8.7 with your version.

##4 - Installing rails 2 and rails 3 RC inside different gemsets

You can separate your rails versions inside gemsets:

**Rails 2:**

Create the gemset:

    $ rvm gemset create rails2

Setting up rvm to use always this gemset:

    $ rvm use 1.8.7@rails2 --default

Installing rails 2:

    $ gem install rails

**Rails 3:**

Create the gemset:

    $ rvm gemset create rails3

Setting up rvm to use always this gemset:

    $rvm use 1.8.7@rails3 --default

Installing rails 3:

    $ gem install rails --pre

##5 - Switching between different rails versions

Switching to rails 2:

    $ rvm use 1.8.7@rails2
    $ rails -v
    Rails 2.3.5

Switching to rails 3:

    $ rvm use 1.8.7@rails3
    $ rails -v
    Rails 3.0.0.rc
