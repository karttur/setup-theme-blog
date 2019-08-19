---
layout: post
title: ImageMagick
previousurl: setup-jekyll-theme-blog
nexturl: null

modified: 2019-08-18T18:17:25.000Z
categories: "blog"
excerpt: Install ImageMagick and edit your images.
tags:
  - ImageMagick
  - macOS
image: avg-rntwi_RNTWI_java_2001-2016_AS
figure1: avg-rntwi_RNTWI_id_2001-2016_large
figure2A: avg-rntwi_RNTWI_id_2001-2016_medium
figure2B: avg-rntwi_RNTWI_java_2001-2016_medium
date: '2018-01-12 16:39'
comments: true
share: true
---

**Contents**
	\- [introduction](#introduction)
	\- [Installation](#installation)
	\- [Basic ImageMagick commands](#basic-imagemagick-commands)
		\- [Change image format and quality](#change-image-format-and-quality)
		\- [Keeping the image format and name](#keeping-the-image-format-and-name)
		\- [Crop image](#crop-image)
		\- [Shave](#shave)
		\- [Set border](#set-border)
		\- [Resize](#resize)
		\- [More functions](#more-functions)
	\- [Batch processing](#batch-processing)
		\- [Combined commands](#combined-commands)
		\- [Terminal Batch processing](#terminal-batch-processing)
	\- [Resources](#resources)

## introduction

[ImageMagick](https://www.imagemagick.org) is a command-line (<span class='app'>Terminal</span>) tool for creating, editing, composing, and converting images. The [ImageMagic site](https://www.imagemagick.org) lists other options than the command-line for accessing the image manipulation functions. In this blog I will only briefly introduce ImageMagick for macOS. The [official homepage](https://www.imagemagick.org), as well as many other blogs, have excellent tutorials and cheatsheets for how to use ImageMagick.

This post focuses on using ImageMagick for batch processing, and describes how I use ImageMagick for editing the maps at the top of Karttur´s GitHib pages.

## Installation

I use [Homebrew](https://brew.sh/), for installing ImageMagick, but you can also go to [ImageMagick Download page](https://www.imagemagick.org/script/download.php) and follow the installation instructions. [Homebrew](https://brew.sh/) is package manager for macOS (other alternatives include MacPorts and Fink, but I prefer Homebrew). If you want to use Homebrew, please visit the [Homebrew site](https://brew.sh/) and install Homebrew.

Start a <span class='app'>Terminal</span> session, and update Homebrew before installing any app to make sure you get the latest version.

<span class='terminal'>$ brew update</span>

Then install ImageMagick from the <span class='app'>Terminal</span> command-line:

<span class='terminal'>$ brew install imagemagick</span>

With older versions of Brew you had to add vector processing for ImageMagick with the command: <span class='terminal'>$ brew install imagemagick --with-librsvg</span>. But this was changed and is no longer required (r even possible).

If you need to remove a Homebrew installed app, use the command _remove_:

<span class='terminal'>$ brew remove imagemagick</span>

Then reinstall the newer version, or install with added commands if requested.
 
## Basic ImageMagick commands

Start a <span class='app'>Terminal</span> session, and go to the directory where you have the images you want to work with.

<span class='terminal'>$ cd /path/to/your/imagefolder</span>

You can also just write <span class='terminal'>$ cd</span> at the command-line and drag the path from a <span class='app'>Finder</span> window.

Make a new director (mkdir), it will be created as a sub-directory under the present <span class='app'>Terminal</span> directory:

<span class='terminal'>$ mkdir pub-images</span>

The syntax in the examples below produces images that are saved in a subfolder <span class='file'>pub-images</span>. If the sub-folder does not exists the ImageMagick functions will not work.

### Important note on file and folder names

The <span class='app'>Terminal</span> command-line interprets all blanks as separators. Thus, if your folder name contains blanks (e.g. my folder) or your file name contains blanks (how to tame a lion.jpg), the <span class='app'>Terminal</span> will not be able to interpret the "commands" my, folder, how, to, tame (lion.jpg might be understood as an image, but one that does not exist). To avoid this you **must** quote folders and files that contain blanks. The the <span class='app'>Terminal</span> thus correctly interprets \"my folder\" and \"how to tame a lion.jpg\".

### Change image format and quality

The basic command for executing ImageMagick is <span class='terminal'>$ convert</span>. All image manipulations require some parameters, except for changing the image format. For changing the image format you only need to state the the source image _SrcImage_  and the destination file where to save the new image, _DstImage_. To change the file type, simply give the desired file type extension in the _DstImage_. If you want to change from, say png to jpg, the command to write in the <span class='app'>Terminal</span> is simply:

 <span class='terminal'>$ convert SrcImage.png pub-images/DstImage.jpg</span>

 As the map images I use at the top of each page are rather large, I wanted to reduce the file size (without changing image dimensions), which I can do using the _quality_ function. All the maps at the top of Karttur´s pages are jpg files, with quality set to approximately 70 %:

  <span class='terminal'>$ convert -quality 70 SrcImage.png pub-images/DstImage.jpg</span>

### Keeping the image format and name

If you want to keep the name of your image after ImageMagick manipulations, you can use <span class='terminal'>$ mogrify</span> instead of <span class='terminal'>$ convert</span>, but you have to give the _-path_ while skipping the _DstImage_ parameter:

 <span class='terminal'>$ mogrify -path pub-images/ SrcImage.ext</span>

 If you do not add any other ImageMagick function, <span class='terminal'>$ mogrify</span> will simply create a copy of _SrcImage.ext_ (where ext is the file format extension). You can always use <span class='terminal'>$ mogrify</span> instead of <span class='terminal'>$ convert</span>, but below I will use <span class='terminal'>$ convert</span>.

### Set border and colors

If you want to have a border on your images, you have to give two  ImageMagick parameters _-border_ and _-bordercolor_:

<span class='terminal'>$ convert -border sidewidthxtopbottomwidth -bordercolor  \"HEXcode\" SrcImage.ext pub-images/DstImage.ext</span>

where _sidewidth_ is the border width at the sides, _topbottomwidth_ the width at the top and bottom, and quoted _HEXcode_ the color code. HEX-code is a standardized hexadecimal system for setting colors as "#RRGGBB". The [w3schools.com site](https://www.w3schools.com) gives a short introduction to [HEX colors](https://www.w3schools.com/colors/colors_hexadecimal.asp), including a tool for [color picking](https://www.w3schools.com/colors/colors_picker.asp). ([w3schools.com](https://www.w3schools.com) is also a generally useful resource for learning html and how to build web-pages). To set a black border width of 2 pixels on all sides, give HEX code = \"#000000\", interpreted as no [00] red, no [00] green and no [00] blue, and then the ImageMagick command for setting the borders (png file type):

<span class='terminal'>$ convert -border 2x2 -bordercolor \"#000000\" SrcImage.png pub-images/DstImage.png</span>

ImageMagick also understands the html accepted standard color names, also listed on the [w3schools.com](https://www.w3schools.com/colors/colors_names.asp) site. Instead of  \"#000000\" you can simply use (unquoted) <span class='terminal'>-bordercolor black</span>. If you want to use transparency, you can instead give colors as quouted "RGBA\(R,G,B,A\)", with the colors (RGB) ranging for 0 to 255, and alpha (A) set as a decimal between 0 (completely transparent) and 1 (completely opaque).

The image below shows a map of Indonesia produced by karttur´s Geo Imagine Framework, with a black frame of 2x2 pixels.

<figure>
<img src="{{ site.commonurl }}/images/{{ site.data.images[page.figure1].file }}">
<figcaption> {{ site.data.images[page.figure1].caption }} </figcaption>
</figure>

### Crop image

The _-crop_ function cuts out a rectangular region. I use _-crop_ to cut out maps to fit the width of my blogs, and to have a wide landscape perspective.

<span class='terminal'>$ convert [+repage] -crop wxh+startx+starty +repage SrcImage.ext pub-images/DstImage.ext</span>

where _w_ = the width of the _DstImage_, _h_ = the height, _startx_ the column in the _SrcImage_ to become the left edge in the _DstImage_, _starty_ the row to become the top edge, and _ext_ is the filetype of _SrcImage_ and _DstImage_. _+repage_ is a special parameters that checks image and canvas areas. It is not always required (denoted by the square parenthesis, []), only some ImageMagick functions require it, and then it depends both on the image type, and how it was created. If the _DstImage_ seems to be shifted or does not look proper, add _+repage_ and try again. The [ImageMagick manual](http://www.imagemagick.org/script/command-line-options.php) tells which functions need the _+repage_ parameter and under what circumstances.

The example below cuts a png image (a large map) to have the dimensions I use for maps at the top of Karttur´s GitHub pages.

<span class='terminal'>$ convert -crop 1280x300+10+10 SrcImage.png pub-images/DstImage.jpg</span>

_DstImage_ will get a size (in pixels) of 1280 columns and 300 rows, starting at column 10 and row 10 in _SrcImage_. The _SrcImage_ image is a png and the result, _DstImage_, will be a jpg image (with no compression, as the _-quality_ is not stated).

### Shave

If the edges of your image contain whitespace or an old frame that you want to remove, you can use _-shave_ instead of _-crop_ to cut out the central part of an image:

<span class='terminal'>$ convert -shave shavesidesxshavetopbottom SrcImage.ext pub-images/DstImage.ext</span>

where _shavesides_ is the width (in pixels) that you want to shave away along the sides, and _shavetopbottom_ the shaving at the top and bottom. The example below shaves 5 pixels at each side, and 10 pixels at the top and bottom (jpg file type):

<span class='terminal'>$ convert -shave 5x10 SrcImage.jpg pub-images/DstImage.jpg</span>

Another options is to use _-trim_ that removes
edges that are the same color as the corner pixels. If you combine _-trim_ with the function _-fuzz_, ImageMagick also removes pixels of nearly the same color (with 'nearness' defined either in absolute or relative terms) as the corners. _-fuzz_ can be used together with several other ImageMagick functions, as explained in the [online manual](http://www.imagemagick.org/script/command-line-options.php).

### Resize

With the ImageMagick _-resize_ function, you can change the dimensions of an image.

<span class='terminal'>$ convert -resize wxh +repage SrcImage.png pub-images/DstImage.png</span>

where _w_ is the _DstImage_ width and _h_ the height.

The image will be proportionally enlarged or reduced to fit the given size. If the given size is not proportional to the _SrcImage_, the _DstImage_ will adopt to the smallest proportional size as default. To force the _-size_ to _w_ and _h_ even if not proportional to the _SrcImage_, add an exclamation mark ("!") after the _-size_ parameter:

<span class='terminal'>$ convert -resize wxh! +repage SrcImage.png pub-images/DstImage.png</span>

In my case (the maps at the top of each page), I wanted the maps to have the same width (1280 pixels), and then I only give the _width_ value:

 <span class='terminal'>$ convert -resize 1280x SrcImage.png pub-images/DstImage.png</span>

To only _-resize_ images larger or smaller than a specific width or height, ImageMagick accept the signs _>_ and _<_, but then you have to quote the _-resize_ parameter. To _-resize_ only images larger than 1280 pixels in width:

<span class='terminal'>$ convert -resize \"1280>x\" SrcImage.ext pub-images/DstImage.ext</span>

Images smaller than 1280 pixels will keep their original size, and _DstImage_ will become an exact copy of _SrcImage_.

You can also apply filters for the _-resize_ command, as explained in the [ImageMagick manual](http://www.imagemagick.org/script/command-line-options.php#resize).

The images below are cropped and resized versions of the Indonesian map above. The command for creating the _-resize_ and _-crop_ of Java is:

<span class='terminal'>convert -resize 1200x -gravity south -crop 596x150-50+5 -border 2x2 -bordercolor black SrcImage.tif -quality 72 pub-images/DstImage.jpg</span>

<figure class="half">
	<img src="{{ site.commonurl }}/images/{{ site.data.images[page.figure2A].file }}" alt="image">
	<img src="{{ site.commonurl }}/images/{{ site.data.images[page.figure2B].file }}" alt="image">
	<figcaption>Two maps cropped from the original map of Indonesia above.</figcaption>
</figure>

How to create the embossed watermark with the text "KARTTUR" is covered in [another post](../add-text-to-image/)

### More functions

ImageMagick has thousands of functions for image manipulations. Once you learn the basics, you will quickly learn to find and use [more functions](http://www.imagemagick.org/script/command-line-options.php).

## Batch processing

As I process a lot of images, both for my scientific work, and for my web.pages, I can not write individual ImageMagick commands for each function and each image. I use two shortcuts, combined commands, and looping over sets of images.

### Combined commands

You can combine ImageMagick functions in a single command, but you must keep the order of functions correct. If you want to have a border, and also change from a png to a jpg image file, with a reduced quality, then you combine the commands like this:

<span class='terminal'>$ convert -border 1x1 -bordercolor black -quality 80 SrcImage.png pub-images/DstImage.jpg</span>

You can expand the command with as many functions you need to create the desired _DstImage_. You can even nest commands for different images within a single command-line, as explained in the [another post](../add-text-to-image/)

### Terminal Batch processing

You can use the <span class='app'>Terminal</span> to set up batch processing, either for single or combined commands. You can then process a whole folder full of images in one go:

<span class='terminal'>$ for i in \*.png; do convert -shave 5x15 +repage -border 2x2 -bordercolor black -resize 1280x -quality 80 \"$i\" \"pub-images/${i%.\*}.jpg\"; done</span>

In the <span class='app'>Terminal</span> command line semi-colon ";" denotes a new command-line function. To write a command over several lines, you instead glue the command at the end of each line with a backslash (\\). Disentangling and commenting ("#") the code above, it can be interpreted as:

```
for i in *.png # Get all files of type png in the present folder and loop while labelling each image i in the loop
    do #starts the loop of the ImageMagick combined functions
    convert -shave 5x15 +repage -border 2x2 -bordercolor "#000000" -resize 1280x -quality 80
    "$i" # is interpreted as the text of the parameter "i" = the filename
    done # ends the loop  
```

## Resources

[Homebrew](https://brew.sh/)

[ImageMagick](https://www.imagemagick.org)

[ImageMagick reference](http://www.imagemagick.org/script/command-line-options.php)

[ImageMagick examples](http://www.imagemagick.org/Usage/)

[Cross platform ImageMagick introduction](http://www.brianlinkletter.com/process-images-for-your-blog-with-imagemagick/) by Brian Linkletter.

[Rubblewebs ImageMagick](http://www.rubblewebs.co.uk/imagemagick/display_example.php?example=49)
