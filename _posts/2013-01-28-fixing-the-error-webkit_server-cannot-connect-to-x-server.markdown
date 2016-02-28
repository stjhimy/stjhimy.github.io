---
layout: post
title:  "'webkit_server: cannot connect to X server'"
date:   2013-01-28
permalink: /posts/31-fixing-the-error-webkit_server-cannot-connect-to-x-server
categories: old
---

tags: Ruby, Rails, Web Development date: 2013-01-16 23:00:00.000000000Z


## What?

If you are using capybara-webkit to do integration tests in a Ubuntu server distribution like Vagrant or a remote dev server, you may see an error like this:

    webkit_server: cannot connect to X server

Don't worry, that's because Ubuntu server don't have an X server running, for those wondering what is a X server:

    "An X server is a program in the X Window System that runs on local machines (i.e., the computers used directly by users) and handles all access to the graphics cards, display screens and input devices (typically a keyboard and mouse) on those computers. "


*******************************************

## Fixing it

We can easily fix it using xvfb to emulate a X server:

    "In the X Window System, Xvfb or X virtual framebuffer is an X11 server that performs all graphical operations in memory, not showing any screen output."

**Installing xvfb**

In your bash:

    $ sudo apt-get update
    $ sudo apt-get upgrade
    $ sudo apt-get install xvfb

**Running the tests**

Now you can run the tests using the xvfb-run command, also you need to specify a DISPLAY:

    $ DISPLAY=localhost:1.0 xvfb-run rake spec

Everything should be good now, you can also add the DISPLAY variable to your .bash_profile:

    export DISPLAY=localhost:1.0

That's it, remember to always run the tests using the xvfb-run command.

Cya.
