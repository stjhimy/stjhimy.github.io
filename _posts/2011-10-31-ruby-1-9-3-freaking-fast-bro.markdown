---
layout: post
title:  "Ruby 1.9.3 freaking fast bro"
date:   2011-10-31
permalink: /posts/24-ruby-1-9-3-freaking-fast-bro
categories: old
---

tags: Ruby, Ruby on Rails date: 2011-10-31 19:37:05.000000000Z


##1.9.3 is here!

I was very excited about this during the day, to finish my work time and test the brand new ruby 1.9.3, and i must say: IT'S AWESOME!

So..Let's see how it goes.


##Installing

    rvm get head
    rvm reload
    rvm install 1.9.3-p0
    rvm use 1.9.3

******************************************************

Just a few minutes and it's done.


##Testing it

There's already a few benchmarks on the web but feels good testing yourself.

##Creating a fresh new project:

*1.9.2:*

    ~/Desktop ruby-1.9.2 $ time rails new test_project --skip-bundle
    real        0m0.473s
    user        0m0.345s
    sys         0m0.117s

    ~/Desktop ruby-1.9.2 $ time rails new test_project2 --skip-bundle
    real        0m0.469s
    user        0m0.333s
    sys         0m0.112s


*1.9.3:*

    ~/Desktop ruby-1.9.3 $ time rails new test_project --skip-bundle
    real        0m0.326s
    user        0m0.250s
    sys         0m0.056s

    ~/Desktop ruby-1.9.3 $ time rails new test_project2 --skip-bundle
    real        0m0.352s
    user        0m0.247s
    sys         0m0.055s

*1.9.3 is faster.*


##Booting

**Fresh new application on rails 3.1:**

*1.9.2:*
    ~/Desktop/test_project ruby-1.9.2 $ time rails runner "puts 'ruby 1.9.2'"
    ruby 1.9.2

    real        0m3.273s
    user        0m2.667s
    sys         0m0.554s

    ~/Desktop/test_project ruby-1.9.2 $ time rails runner "puts 'ruby 1.9.2'"
    ruby 1.9.2

    real        0m3.273s
    user        0m2.660s
    sys         0m0.559s


*1.9.3:*

    ~/Desktop/test_project ruby-1.9.3 $ time rails runner "puts 'ruby 1.9.3'"
    ruby 1.9.3

    real        0m1.919s
    user        0m1.655s
    sys         0m0.241s

    ~/Desktop/test_project ruby-1.9.3 $ time rails runner "puts 'ruby 1.9.3'"
    ruby 1.9.3

    real        0m1.857s
    user        0m1.611s
    sys         0m0.235s

*1.9.3 is faster.*


**Huge app containing a lot of dependencies on rails 3.0.10:**

*1.9.2:*

    ~/Desktop/huge_app ruby-1.9.2 $ time rails runner "puts 'ruby 1.9.2'"
    ruby 1.9.2

    real        0m10.439s
    user        0m9.020s
    sys         0m1.286s

    ~/Desktop/huge_app ruby-1.9.2 $ time rails runner "puts 'ruby 1.9.2'"
    ruby 1.9.2

    real        0m10.447s
    user        0m9.035s
    sys         0m1.300s

*1.9.3:*

    ~/Desktop/huge_app ruby-1.9.3 $ time rails runner "puts 'ruby 1.9.3'"
    ruby 1.9.3

    real        0m5.579s
    user        0m4.827s
    sys         0m0.697s

    ~/Desktop/huge_app ruby-1.9.3 $ time rails runner "puts 'ruby 1.9.3'"
    ruby 1.9.3

    real        0m5.793s
    user        0m4.942s
    sys         0m0.704s

*1.9.3 is way faster.*


##rake spec on the same app:


*1.9.2:*

    ~/Desktop/huge_app ruby-1.9.2 $ time rake spec

    real        0m29.737s
    user        0m25.503s
    sys         0m3.197s


    ~/Desktop/huge_app ruby-1.9.2 $ time rake spec

    real        0m29.693s
    user        0m25.348s
    sys         0m3.196s


*1.9.3:*

    ~/Desktop/huge_app ruby-1.9.3 $ time rake spec

    real        0m17.306s
    user        0m14.136s
    sys         0m1.761s


    ~/Desktop/huge_app ruby-1.9.3 $ time rake spec

    real        0m16.412s
    user        0m13.965s
    sys         0m1.724s

*1.9.3 is way way faster.*

**/lost image//**
