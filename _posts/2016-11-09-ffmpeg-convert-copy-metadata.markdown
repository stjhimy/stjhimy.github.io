---
layout: post
title:  ffmpeg convert and copy metadata
date:   2016-11-09
categories: #ffmpeg #linux
---

For a list of `.mp4` files in a directory:

```
for i in $( ls ); do ffmpeg -i $i -filter:v \
  "setpts=0.003*PTS" -threads 4 -an -map_metadata 0\
  -map_metadata:s:v 0:s:v -map_metadata:s:a 0:s:a _$i\
  && exiftool "-filemodifydate<mediacreatedate" _$i; done
```
