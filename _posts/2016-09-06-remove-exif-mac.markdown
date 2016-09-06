---
layout: post
title:  Remove exif data
date:   2016-09-06
categories: #bash
---

## Install Imagemagick

Mac:

```bash
brew install imagemagick
```

Ubuntu:

```bash
apt-get install imagemagick
```

## Bash it

```
convert --help | grep strip
  -strip strip image of all profiles and comments
```

```bash
for i in $( ls ); do convert $i -strip $i; done 
```
