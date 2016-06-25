---
layout: post
title:  Change macOS default screenshot folder
date:   2016-06-13
categories: #macos #osx
---

Note to all next installations:

{% highlight bash%}
defaults write com.apple.screencapture location ~/Downloads
killall SystemUIServer
{% endhighlight %}

Bonus: Reset the dock

{% highlight bash%}
defaults delete com.apple.dock; killall Dock
{% endhighlight %}
