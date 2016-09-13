---
layout: post
title:  Fix usb drive after dd
date:   2016-09-13
categories: #off-topic
---

> dd is a command-line utility for Unix and Unix-like operating systems whose primary purpose is to convert and copy files.[1]

[https://en.wikipedia.org/wiki/Dd_(Unix)](https://en.wikipedia.org/wiki/Dd_(Unix))

dd is also used to make a usb flash drive bootable using an image (.iso, etc).

To make the usb stick usable again you must recreate the partition using a `Master Boot Record`, can be easily achieved with **Disk Utility**:

<a href="http://imgur.com/BzZEEZz"><img src="http://i.imgur.com/BzZEEZz.png" title="source: imgur.com" /></a>

Now the usb stick will be detected/usable on any machine (Windwos/Unix).
