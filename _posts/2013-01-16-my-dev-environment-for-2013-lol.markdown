---
layout: post
title:  "My dev environment for 2013 lol"
date:   2013-01-16
permalink: /posts/30-my-dev-environment-for-2013-lol
categories: old
---

tags: Ruby, Rails, Web Development date: 2013-01-16 21:00:00.000000000Z

Hello Folks, I would like to share my current DEV environment for Ruby/Rails Fulltime. There is nothing too fancy, just a simple and productive environment.

## Macbook pro mid 2010

![](https://www.evernote.com/shard/s120/sh/14fb27c8-d81d-4ce1-bf2f-d96aba3f4479/9ca832a4b6cac856976146613d7f5188/res/59f57ddf-1b41-48ee-8db7-cea5a20c35ec/skitch.png)

Yes, i'm still coding on a Macbook pro mid 2010, why? It's enough! It is not the fastest machine on earth but it is enough for ruby/rails/web development in general, i use it mostly for work/feeds/internet-shit. Gamming? Forget about it, i use my iPhone 4S and my xbox to do that.

********************************

![](https://www.evernote.com/shard/s120/sh/34216851-562a-4dc6-b566-ae11f5ec9f4c/4bbc964879757904864c3c9cee826948/res/1b5fd1fb-cdb8-4b99-8fd2-b4ef59e04da5/skitch.png)

## Vagrant

![](https://www.evernote.com/shard/s120/sh/da6f3d66-a5a7-4e72-ab8b-fd73c87a58c3/25145fef10c57e8c8d3dfaaf34d58068/res/c9ca401d-10e9-44f2-9749-022896803322/skitch.png)

I've started using vagrant a few weeks ago, and i got say, it's awesome. I'm running an Ubuntu 12.04 virtual machine, 512 ram.

**Inside vagrant**

    Text Editor - Vim

    Rvm
        Ruby 1.9.3
        Ruby 1.8.7

    Dbs
        Sqlite
        Postgresql
        Mysql
        Mongodb
        Redis

I use vim inside vagrant only for quicking editing files or git commits.

**tmux**

If you don't know tmux yet, it makes managing shell sessions really easy, you can split, create new windows, create multiple sessions and stuff.

[http://tmux.sourceforge.net/](http://tmux.sourceforge.net/)

![](http://screencloud.net/img/screenshots/8fc6a4ce3b5ccd7ade85e9b542bf92d3.png)

Also there's a great gem to manage complex tmux sessions (which window should open for each project), it's called tmuxinator, check this out here: [https://github.com/aziz/tmuxinator](https://github.com/aziz/tmuxinator)

## Text Editor - Macvim for the rescue

Tried lots of different editors, Sublime, TextMate, vim on bash but my heart always goes back to MacVim.

[http://code.google.com/p/macvim/](http://code.google.com/p/macvim/)

![](http://screencloud.net/img/screenshots/6aed415cd087edc5bb3769799dc5ce69.png)

There's a pack with lots of snippets and plugins, you can find it here:
[https://github.com/akitaonrails/vimfiles](https://github.com/akitaonrails/vimfiles)

## Browser - Chrome

My default browser is Chrome (OHH BIG NEWS!) but i keep a virtual machine with Chrome/Firefox/Internet Explorer so i can test in multiples browsers...specially Internet Explorer, meh.

![](http://screencloud.net/img/screenshots/83b3b6348d497614cd5d38fd68ba28bc.png)


That's it, not a fancy environment, give us a bash, a text editor and a browser and we can do anything :-)
