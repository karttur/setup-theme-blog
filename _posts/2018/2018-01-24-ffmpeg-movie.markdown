---
layout: post
title: Create FFmpeg movie
previousurl: null
nexturl: null
modified: '2018-01-23 09:14'
categories: blog
excerpt: Create movie with time line and clock for a time series of image maps
tags:
  - FFmpeg
  - ImageMagick
  - animation
  - mp4
  - H.264
image: avg-twi-percent_MCD43A4_oka_2001-2016_A
date: '2018-01-24 01:04'
comments: true
share: true
figure1: avg-twi-percent_MCD43A4_bw_2001-2016_A
figure2: twi-percent_MCD43A4_oka-makgadik_201012_text
figure3: imageclock_oka-makgadik_201012
movie1: twi-percent_MCD43A4_oka-makgadik_2001-2016_v005-twi01-MS
---

## Introduction

In this post I will use a time series of monthly satellite observations to create an animation of the soil water content over the Okavango swamps and the Makgadikgadi pans in northern Botswana. The satellite maps are produced by Karttur´s Geo Imagine Framework, and overlaid by a time line and an annual clock that shows the time in the animation. The images for the animation are prepared using [ImageMagick](https://www.imagemagick.org/) and the movie is created using [FFmpeg](https://www.ffmpeg.org/). [How to install ImageMagick is covered in and earlier post](../install-imagemagick/), and how to put text and compose images is covered in [another post](../add-text-to-image/).

## Install FFmpeg

I am using [FFmpeg](https://www.FFmpeg.org/), a <span class='app'>Terminal</span> command-line driven "cross-platform solution to record, convert and stream audio and video". If you do not have FFmpeg installed, you can either download FFmpeg from the [official homepage](https://www.FFmpeg.org/) or use Homebrew, introduced in an [earlier post](../install-imagemagick/).

<span class='terminal'>$ brew install ffmpeg</span>

FFmpeg can do vastly more than covered in this post, and there are many manuals and tutorials on how to install, configure and use FFmpeg out there. Here I will only use FFmpeg for creating an animation from a sequence of still images.

## Image sequence

When creating an animation you need a sequence of images with the same dimensions (width and height). You can either prepare the images to have the dimensions of the final movie, or use FFmpeg to set the movie dimensions. I prefer to prepare the images with ImageMagick as that gives better control.

The maps that I (Karttur) produce are not primarily intended for movies. And even if Karttur's Geo Imagine Framework can be set to produce maps of any dimensions over any part of the globe, I prefer to keep fewer regions in the Framework and use ImageMagick to prepare image maps for animations.

### On Karttur's map naming convention

All map files imported or produced from Karttur's Geo Imagine Framework have a strict naming convention. Each image file name contains 5 main parts, where each part can also contain sub-parts.
The main parts are separated by underscore (“\_”) and include:
* content (indicating the thematic content)
* source (indicating the source of the content)
* location (indicating the geographical location)
* date (indicating the temporal validity of the content)
* suffix (any additional information, including version)

The general name form is thus:

content_source_locations_date_suffix.ext

Each part can be further divided using hyphen (“-”).

Data representing a specific country use the country 2-letter iso-code for representing location. Data with a date given as 4 integers represent annual data, 6 integers represent year+month, 7 integers year+day of year and 8 integers date represent the form YYYYMMDD. Dates including a hyphen represent a time span with the first item (before the hyphen) representing the initial date and the last item the last date.

### ImageMagick image preparation

This section assumes that you have a sequence of images in a separate folder. And that the image files have a naming convention that allows the sequence to be recognized. The maps I am using in this post are over the Okavango swamps and the Makgadikgadi pans in Botswana (iso-code: bw). The thematic content is soil moisture estimated from Karttur's "Transformed Wetness Index" (TWI) expressed as volume water over total volume and converted to percentage. The source of the data is MODIS satellite images (product MCD43A4, version 005). I use average monthly (MS) data for the animation in this post. The image file names thus become:

twi-percent_MCD43A4_bw_YYYYMM_v005-twi01-MS.tif

where YYYY and MM indicate the year and month represented in the image. The image below shows the average soil moisture (volume/volume) for Botswana 2001-2016, and is created using ImageMagick as explained in [another post](../install-imagemagick/) with an added embossed watermark as described in [this post](../add-text-to-image/). The ImageMagick <span class='app'>Terminal</span> command for creating the image below from the original map is (with capital "C" replace by "("):

<span class='terminal'>$ convert \\C -resize 716x -border 2x2 -bordercolor black SrcImage.tif \\) \\C -size 716x150 xc:none -font Trebuchet -pointsize 150 -gravity center -draw \"fill silver text 1,1 \'KARTTUR\'  fill whitesmoke text -1,-1 \'KARTTUR\' fill grey text 0,0 \'KARTTUR\' \" -transparent grey -fuzz 90% \) -composite -quality 72 DstImage.jpg</span>

The image width then becomes 720 pixels (716 + 2*border), which is the width of old analogue television sets (720x576). I will use that dimension for creating an animation in this post.

<figure>
<img src="{{ site.commonurl }}/images/{{ site.data.images[page.figure1].file }}">
<figcaption> {{ site.data.images[page.figure1].caption }} </figcaption>
</figure>

I do, however, want to create an animation only of the Okavango swamps and the Makdagikgadi pans in northern Botswana. I must thus first _-resize_ and _-crop_ my image sequence to fit the movie dimensions I chose.

### Resize and crop images in time series

Once you have decided which region in your map (image) to use for creating a movie, use ImageMagick to _-resize_ and _-crop_ your images. Do not compress the images, but keep the png format throughout. The compression will be set in the move with FFmpeg. I also put the embossed watermark text on my films, so my ImageMagick command for getting the image sequence I want is (again you have to replace capital "C" with "("):

<span class='terminal'>$ for i in \*.tif; do convert \\C -resize 1450x -crop 720x576+250+55 \"$i\" \\) \\C -size 720x150 xc:none -font Trebuchet -pointsize 100 -gravity center -draw \"fill silver text 1,1 \'KARTTUR\'  fill whitesmoke text -1,-1 \'KARTTUR\' fill grey text 0,0 \'KARTTUR\' \" -transparent grey -fuzz 90% \\) -composite  \"pub-movies/${i%.\*}.png\"; done</span>

The image below shows one frame in the movie, prior to putting the clock on top. The text in the map is created using ImageMagick:

<span class='terminal'>$ convert SrcImage.png
-size 225x50 -background none -font CourierNewI -pointsize 24 -fill blue
caption:\"Okavango\" -geometry +170+220 -composite
caption:\"Lake Ngami\" -geometry +100+375 -composite
caption:\"Makgadikgadi Pans\" -geometry +500+350 -composite
caption:\"Linyanti\" -geometry +240+64 -composite
-quality 72 DstImage.jpg</span>

<figure>
<img src="{{ site.commonurl }}/images/{{ site.data.images[page.figure2].file }}">
<figcaption> {{ site.data.images[page.figure2].caption }} </figcaption>
</figure>

## ImageClock

The ImageClock is created from a Python package that I wrote, and is part of Karttur's Geo Imagine Framework. It is not publicly available. The trick is that it produces image clocks with files named exactly corresponding the image maps, but in a separate folder. Below is the <span class='package'>ImageClock</span> file for the image frame above (black is set to transparent in the overlay, but shown in the image below).

<figure>
 <img  title="{{ site.data.images[page.figure3].credit }}" src="{{ site.commonurl }}/images/{{ site.data.images[page.figure3].file }}">
<figcaption> <a href="{{ site.commonurl }}/images/{{ site.data.images[page.figure3].source }}">{{ site.data.images[page.figure3].caption }}</a> </figcaption>
</figure>

## Overlay clock on map

You can put the clock above of below the map, but here I put it at the bottom, with the clock to the left. To overlay a single clock over a single map, the general ImageMagick command is:

<span class='terminal'>$ convert SrcImage.png ClockImage.png -composite DstImage.png</span>

Putting the clock in the lower left corner, add _-gravity_ and set it to _southwest_

<span class='terminal'>$ convert SrcImage.png ClockImage.png -gravity southwest -composite DstImage.png</span>

To loop all images, use <span class='app'>Terminal</span> batch processing (introduced in [this post](../install-imagemagick/index.html#terminal-batch-processing)). This is fairly easy because the map image and the overlay clock have the same name, but are in different folders.

<span class='terminal'>$ for i in \*.png; do convert \"$i\" \"../imageclock/$i\" -gravity southwest -composite  \"movieframes/$i\"; done</span>

You should now have a set of png images, each with a map representing a time step in a time series, with a clock and time line overlaid that shows the time of the map frame. It is time to create a movie, for which you will use FFmpeg.

## Create FFmpeg movie

The basic FFmpeg command for creating a movie from a set of still images is:

<span class='terminal'>$ ffmpeg -f image2 -pattern_type glob -i \'\*.png\' DstMovie.avi</span>

This renders an avi movie using the standard frame rate (frames per second, fps) of 25. That is a bit too fast for a map animation. And if you try the ffmpeg command above, you will also see that avi movie is of rather low quality. To create a more pleasing movie, you need to define fps and quality more carefully. Set the frame rate (fps) with the option _-r_:

<span class='terminal'>$ ffmpeg -r 5 -f image2 -pattern_type glob -i \'\*.png\' DstMovie.avi</span>

### Set codec and compression

As the created animation is intended for publication over the internet, you need to select a [film format that can be read by web broswer](https://developer.mozilla.org/en-US/docs/Web/HTML/Supported_media_formats). At time of writing this post, <span class='file'>.mp4</span> and <span class='file'>.webm</span> are the most widely accepted formats. Both mp4 and webm are containers, and the actual coding inside can be set in a myriad of ways. I chose to use mp4 and the H.264 codec. The basic FFmpeg command then becomes:

<span class='terminal'>$ ffmpeg -r 3 -f image2 -pattern_type glob -i \'\*.png\' -vcodec libx264 -pix_fmt yuv420p -crf 20 DstMovie.mp4</span>

where _-vcodec_ _libx264_ is the H.264 video codec, _-pix_fmt_ specifies the pixel color coding resampling (_yuv420p_ is widely used for H.264 movies) _-crf_ is the quality. To reduce the file size you can set a lower quality, by increasing _-crf_. The quality for an animated map can be set rather low, so I will go for _-crf 35_ to reduce the file size. You can also set a target bitrate (the data transfer per second) by using the _-b_ option. For my movie I set the target bitrate to 1M (1000K), and the animation shown below is created with following ffmpeg command:

<span class='terminal'>$ ffmpeg -r 3 -f image2 -pattern_type glob -i \'\*.png\' -vcodec libx264 -b:v 1M -pix_fmt yuv420p -crf 35 DstMovie.mp4</span>


<figure>
<iframe src="{{ site.commonurl }}/movies/{{ site.data.movies[page.movie1].file }}" width="{{ site.data.movies[page.movie1].width }}" height="{{ site.data.movies[page.movie1].height }}" frameborder="0">
</iframe>
<figcaption> {{ site.data.movies[page.movie1].caption }} </figcaption>
</figure>

You can increase the compatibility of the movie by adding the option _-profile:v baseline_. For me that almost doubled the file size. As this animation is intended for use by web browser, the published version does not have the _-profile:v baseline_.

<span class='terminal'>$ ffmpeg -r 3 -f image2 -pattern_type glob -i \'\*.png\' -vcodec libx264 -b:v 1M -pix_fmt yuv420p -profile:v baseline -crf 35 DstMovie.mp4</span>

## Resources

[FFmpeg](https://www.ffmpeg.org)
