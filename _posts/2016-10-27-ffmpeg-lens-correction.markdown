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

## Updated - 2016-10-29

Here's a small script to generate sequential corrected images using k1/k2 incremented by 0.1:

```ruby
(-1.0..1.0).step(0.1).map{|e| e.round(2)}.map do |k|
  range.map{|v| [k,v]}
end.reduce(:+).each_with_index do |value, index|
  k1, k2 = value
  system("ffmpeg -i in.jpg -vf \
         lenscorrection=k1=#{k1}:k2=#{k2} \
         IMG_#{index}.JPG &> /dev/null")
end
```

input -> in.jpg

output -> IMG_1.JPG, IMG_2.JPG, IMG_3.JPG

Now you have a list of  corrected images that can be used to determine the best values for k1 and k2.

## Updated - 2016-10-29 - ffmpeg vs photoshop


ffmpeg:
<a href="http://imgur.com/WZ9awlF"><img src="http://i.imgur.com/WZ9awlF.jpg" title="source: imgur.com" /></a>

Photoshop:
<a href="http://imgur.com/PQkRm5d"><img src="http://i.imgur.com/PQkRm5d.jpg" title="source: imgur.com" /></a>

As you see the values for k1 and k2 are not perfect, the corners still rounded.
