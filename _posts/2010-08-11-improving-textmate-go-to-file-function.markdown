---
layout: post
title:  "Improving TextMate 'Go To File' function"
date:   2010-08-11
permalink: /posts/12-improving-textmate-go-to-file-function
categories: old
---

tags: Tips and Tricks date: 2010-08-11 20:19:57.000000000Z

##Go to file is just one of the most helpful tools.

I've been working with TextMate for almost a month now, and there's a thing that i just hate:
When you use "Go To File (Command + T)" you are not able to specify the folder of the file,
in a small project you barely note that, but when things get huge it makes a big difference.

To make it clear, let's think you have a project with 2 scaffolds, cars and bikes.
If you try to find the "index.html" using the Default "Go To File", you will see that:

![](https://img.skitch.com/20110906-ndrb4rujd3xgieptt598j89y8s.jpg)

You just see a list of all files called "index", but what if you want to specify the cars/index?
Sounds useless when you have 2 or 3 index...but you will love that when your project gets huge.
I just found a bundle for TextMate in github for that, to install is pretty easy:

    cd ~/Library/Application\ Support/TextMate/Bundles/
    git clone git://github.com/amiel/gotofile.tmbundle.git GoToFile.tmbundle

Restart your TextMate, to open the new go to file press "Command + Shift + K":

![](https://img.skitch.com/20110906-x1p4jft6qb7i5attp7em86n52f.jpg)


If you prefer take a look at [github](http://github.com/amiel/gotofile.tmbundle), and if you have a better solution, please let me know.

##Edited

There's also peepopen from [PeepCode](http://peepcode.com/products/peepopen).
It's a great plugin for just $9.

![](https://img.skitch.com/20110906-cf68qy1fdx12bjm5h4ufrm6x8e.jpg)
