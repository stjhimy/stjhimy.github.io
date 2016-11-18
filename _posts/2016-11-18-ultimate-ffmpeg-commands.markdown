---
layout: post
title:  ffmpeg helpful commands
date:   2016-11-18
categories: #off-topic
---

Convert all files in a directory to timelapses and copy metadata:

```
for i in $( ls ); do ffmpeg -i $i -filter:v "setpts=0.003*PTS" -threads 0 -an -map_metadata 0 -preset ultrafast -map_metadata:s:v 0:s:v -map_metadata:s:a 0:s:a _$i && exiftool "-filemodifydate<createdate" _$i; done
```

Convert all files in a directory and copy metadata:

```
for i in $( ls ); do ffmpeg -i $i  -threads 0 -crf 33 -vf scale=1280:720 -preset ultrafast -map_metadata 0 -map_metadata:s:v 0:s:v -map_metadata:s:a 0:s:a _$i && exiftool "-filemodifydate<createdate" _$i; done
```

Extract screenshots from video:

```
ffmpeg -i input.mp4 -vf fps=0.5 -map_metadata 0 -map_metadata:s:v 0:s:v -map_metadata:s:a 0:s:a out%d.jpg
```
