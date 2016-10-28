---
layout: post
title:  ffmpeg lens correction
date:   2016-10-27
categories: #off-topic
---

Example image:

<a href="http://imgur.com/7hkkW67"><img src="http://i.imgur.com/7hkkW67.jpg" title="source: imgur.com" /></a>

> This filter can be used to correct for radial distortion as can result from the use of wide angle lenses,
 and thereby re-rectify the image. To find the right parameters one can use tools available for example as part
 of opencv or simply trial-and-error. To use opencv use the calibration sample (under samples/cpp) from the opencv sources and extract the k1 and k2 coefficients from the resulting matrix.

 [http://www.ffmpeg.org/ffmpeg-all.html#lenscorrection](http://www.ffmpeg.org/ffmpeg-all.html#lenscorrection)

> k1 - Coefficient of the quadratic correction term. 0.5 means no correction.

> k2 - Coefficient of the double quadratic correction term. 0.5 means no correction.

```bash
ffmpeg -i in.jpg\
   -vf lenscorrection=k1=-0.56:k2=0.3\
   out.jpg
```

Output image:

<a href="http://imgur.com/WZ9awlF"><img src="http://i.imgur.com/WZ9awlF.jpg" title="source: imgur.com" /></a>

## Updated - 2016-10-28

This guy used a dynamic script to find the most suitable values for k1 and k2, might be helpful:
[https://www.youtube.com/watch?v=hieagk2l4lI](https://www.youtube.com/watch?v=hieagk2l4lI)
