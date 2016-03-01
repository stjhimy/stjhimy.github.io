---
layout: post
title:  "Fix 'encode': \\xC3 on US-ASCII ..."
date:   2015-09-14
categories: old
---

Sometimes in a newly created server you can get this error when starting a ruby app:

    `encode': "\xC3" on US-ASCII (Encoding::InvalidByteSequenceError)

To fix simple export this variables or add to your ~/.bash_profile

    export LANG=en_US.UTF-8
    export LC_ALL=en_US.UTF-8

Don't forget to 'source ~/.bash_profile'
