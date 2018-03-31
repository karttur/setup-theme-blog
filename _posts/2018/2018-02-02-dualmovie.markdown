---
layout: post
title: Animation with inset map
previousurl: null
nexturl: null
modified: '2018-02-02 09:14'
categories: blog
excerpt: 'Create animation with time series images, small inset map and timeline/clock'
tags:
  - FFmpeg
  - ImageMagick
  - animation
  - inset
image: avg-twi-percent_MCD43A4_oka_2001-2016_A
figure1A: twi-percent_MCD43A4_oka-basin_200409_twi
figure1B: twi-percent_MCD43A4_oka-basin_200409_rainfall
figure1C: twi-percent_MCD43A4_oka-basin_200409_clock
figure1D: oka-basin-border
figure2: twi-percent_MCD43A4_oka-basin_200409_v005-movieframe
movie1: twi-trmm-clock-basin_mp4_oka-basin_2001-2016_MS
date: '2018-02-02 11:54'
comments: true
share: true
---
**Contents**
	\- [introduction](#introduction)
	\- [Prerequisites](#prerequisites)
	\- [Image organization](#image-organization)
	\- [Animation frames](#animation-frames)
	\- [Movie](#movie)
	\- [Resources](#resources)

## introduction

The [previous post](../ffmpeg-movie/) introduced FFmpeg for creating an animation from satellite time series images. In this post I will show how to use ImageMagick, introduced in an [earlier post](../install-imagemagick/) and FFmpeg for creating an animation combining two time series and an image clock.

## Prerequisites

To create an animation with a large map at the bottom and overlaid by a smaller inset map, you need the command line apps <span class='app'>ImageMagick</span> and <span class='app'>FFmpeg</span>. You also need at least two time series of images. The main image time series will decide the time span and the time resolution of the animation. The second (slave) time series must have images covering the same time span and temporal resolution. If the slave time series contain additional data they will be ignored.

## Image organization

The time series data to use for creating the animation shown below must be organized in a way that allows the time corresponding master and slave(s) images to be recognized. This can, for instance be done using a programming language, or by a very strict naming of all images.

All the images used in this post are produced from Karttur's Geo Imagine Framework. At the time of writing I have not made the parts of Framework needed publicly available. But the essence is that the Framework produces all the time series images and saves them in separate folders, *with identical file names*. With both the master image time series and all slave time series having exactly the same file name for each time stamp, the animation can be created using only command-line calls to ImageMagick and FFmpeg.

## Animation frames

The animation created in this post is a composition of the following elements:

* Time series of soil moisture images
* Time series of rainfall images
* A time line and clock time series of images
* A vector map of the river basin

<figure class="half">
	<a href="{{ site.commonurl }}/images/{{ site.data.images[page.figure1A].source }}"><img src="{{ site.commonurl }}/images/{{ site.data.images[page.figure1A].file }}" alt="image"></a>

  <a href="{{ site.commonurl }}/images/{{ site.data.images[page.figure1B].source }}"><img src="{{ site.commonurl }}/images/{{ site.data.images[page.figure1B].file }}" alt="image"></a>

  <a href="{{ site.commonurl }}/images/{{ site.data.images[page.figure1C].source }}"><img src="{{ site.commonurl }}/images/{{ site.data.images[page.figure1C].file }}" alt="image"></a>

  <a href="{{ site.commonurl }}/images/{{ site.data.images[page.figure1D].source }}"><img src="{{ site.commonurl }}/images/{{ site.data.images[page.figure1D].file }}" alt="image"></a>
	<figcaption>Images used for composing animation frames, the upper row shows soil moisture and rainfall, the lower row the timeline/clock and the river Basin. The geographical region is the Okavango River basin in southern Africa. If you click on the images you will see them with the input dimensions used for creating the animation.</figcaption>
</figure>

I want the elements to be layered with:

* soil moisture as the main image,
* rainfall as an inset in the upper right corner,
* time line and clock at the lower edge,
* both soil moisture and rainfall overlaid with the river basin vector, and
* embossed watermark for the final frame.

<figure>
<img src="{{ site.commonurl }}/images/{{ site.data.images[page.figure2].file }}">
<figcaption> {{ site.data.images[page.figure2].caption }} </figcaption>
</figure>

With the soil moisture (master) images and the slave images (rainfall and timeline/clock) having identical file names, the ImageMagick command for creating the frames for the animation becomes:

```
mkdir frames
for i in *.tif; do convert \( -resize 992x1080! "$i" \) \
\( -resize 992x1080! -background none /path/to/riverbasinvector/river-basin-vector.svg \) -composite \
\( \( -resize 380x -border 1x1 -bordercolor black /path/to/rainfallimages/"$i" \) \
\( -resize 380x -background none /path/to/riverbasinvector/river-basin-vector.svg \) -gravity center -composite \) \
-gravity northeast -composite \
/path/to/timelineclockimages/${i%.*}.png -gravity southwest -composite \
\( -size 992x300 xc:none -font Trebuchet -pointsize 200 -gravity center -draw "fill silver text 1,1 'KARTTUR'  fill whitesmoke text -1,-1 'KARTTUR' fill grey text 0,0 'KARTTUR' " -transparent grey -fuzz 90% \) -composite   "frames/${i%.*}.png"; done
```

For the mp4 _-vcodec_ _libx264_ to work with the pixel formatter _-pix_fmt_ _yuv420p_, both the width and height of the input must be an even number. In the ImageMagick command above I thus forced the dimensions of the master image to even numbers (_-resize 992x1080!_).

## Movie

With the frames ready, the movie is created using FFmpeg as suggested in the [previous post](../ffmpeg-movie/). But as the example movie in this post has larger dimensions, I increased the bitrate to 2M.

```
ffmpeg -r 3 -f image2 -pattern_type glob -i '*.png' -vcodec libx264 -b:v 2M -pix_fmt yuv420p -crf 35 DstMovie.mp4
```

The movie has a dimension of 992x1080 (approximately 1 km spatial resolution on the ground for the soil moisture). The dimensions when shown below is reduced to 716x780 to fit neatly to the layout in this blog. If you download the movie, and view it on your local machine it will have the original dimensions of 992x1080. (The original data is at approximately 500 m spatial resolution, and a movie at full resolution would thus be 1986×2164).

<figure>
<iframe src="{{ site.commonurl }}/movies/{{ site.data.movies[page.movie1].file }}" width="{{ site.data.movies[page.movie1].width }}" height="{{ site.data.movies[page.movie1].height }}" frameborder="0">
</iframe>
<figcaption> {{ site.data.movies[page.movie1].caption }} </figcaption>
</figure>

## Resources

[ImageMagick](https://www.imagemagick.org)

[FFmpeg](https://www.ffmpeg.org/)

[Create FFmpeg movie](../ffmpeg-movie/) my previous post.
