---
layout: post
title: ImageMagick text display
modified: 2018-01-07T18:17:25.000Z
categories: blog
excerpt: Add alphanumeric text to images
tags:
  - ImageMagick
  - image text
  - image label
  - image caption
  - time-series animation
image: avg-trmm-fao-surplus-vwb_vwb_trmm_2001-2016_A
date: '2018-01-13 22:50'
comments: true
share: true
---

**Contents**
	\- [Introduction](#introduction)
		\- [Options for setting text](#options-for-setting-text)
		\- [Annotate](#annotate)
			\- [Font size and dpi](#font-size-and-dpi)
			\- [Position anchor (_-gravity_)](#position-anchor-gravity)
			\- [Font color](#font-color)
			\- [Font type](#font-type)
		\- [Caption](#caption)
	\- [Composite](#composite)
	\- [Resources](#resources)

## Introduction

Putting a caption, label, watermark or other text directly in an image for web publishing is common. I also wanted to create animations from time-series of satellite images and model results, and put both a caption, a time line and a clock in the image. This can not be done manually, and I chose to use ImageMagick. This post only covers how to use [ImageImagick](https://www.imagemagick.org) for putting text in images, and to create image compositions. Other posts in other blogs cover the creation of the images, the time line and the clock. An earlier post within this blog includes an [introduction to ImageMagick](../2018/2018-01-13-install-imagemagick.html).

### Options for setting text

The ImageMagick functions _-annotate_, _-draw_, _-caption_ and _-label_  can all be used for writing text to images. Which function to use depends on how much and what type of control you want to have over the text that is written. The general options for setting fonts, colors and postions are similar. The blog uses _-annotate_ as the vehicle for explaining how to put text in images, and then I describe how I finally ended up using either _-caption_ or _-label_, and why.

### Annotate

The basic command-line syntax to _-annotate_ an image using ImageMagick is:

<span class='terminal'>$ convert -annotate +startx+starty "annotation" SrcImage.ext DstImage.ext</span>

where _startx_ is the image column where the text should start, _starty_ is the row where to start, and the quoted text that follows (_annotation_) is the text to write; _SrcImage_ and _DstImage_ are the source and destination images, with _ext_ denoting the image file type. If you want to write "My map" in the upper left corner (20 pixels from the left and 30 pixels from the top) on a png file, this then becomes:

 <span class='terminal'>$ convert -annotate +20+30 "My map" SrcImage.png DstImage.png</span>

#### Font size and dpi

To define the font size, add the function _-pointsize_ before _-annotation_:

<span class='terminal'>$ convert -pointsize points -annotate +startx+starty "annotation" SrcImage.ext DstImage.ext</span>

where the parameter _points_ is the font size in dots per inch (dpi). How big your fonts are going to become then depends on the dpi that your image is set to. You can set, or change, that using ImageMagick. dpi does not really matter for the internet, as modern web-browser ignore the dpi setting, but traditionally web images have been set to 72 dpi. For printing the most common is usually 300 or 600 dpi (or somewhere in between).

You can use ImageMagick to query the image metadata (data about the image):

<span class='terminal'>$ magick identify SrcImage.ext</span>

which will return the most basic metadata, but not the dpi. You can add different parameters to extend the returned metadata, as described in the [ImageMagick manual](https://www.imagemagick.org/script/identify.php). One option is the parameters _-format_, and to get the image size and resolution (dpi) execute the <span class='app'>Terminal</span> command:

<span class='terminal'>$ magick identify -format "%w x %h %x x %y" SrcImage.ext</span>

You then get the width and height (in pixels) and the horizontal and vertical dpi. If you want to change the dpi, use the ImageMagick function _-units_, with either the parameter _-density_ or _-resample_. The latter has more advanced options available - see the [ImageMagick manual](). To keep the image size, use _-density_ :

<span class='terminal'>$ convert -units PixelsPerInch SrcImage.ext -density dpi DstImage.ext</span>

where _dpi_ is the dpi you want the _DstImage_ to have. Do not change _PixelsPerInch_, it defines the _-units_ to change. To change a png image to have 72 dpi, the command becomes:

<span class='terminal'>$ convert -units PixelsPerInch SrcImage.png -density 72 DstImage.png</span>

If you try the same font size (expressed as _-pointsize_) on two copies of the same image, one at 72 dpi and the other at 300, you will get differently sized text on the two images.

 <span class='terminal'>$ convert -pointsize 30 -annotate +20+30 "My map" SrcImage@72dpi.ext DstImage@72dpi.ext</span>

<span class='terminal'>$ convert -pointsize 30 -annotate +20+30 "My map" SrcImage@300dpi.ext DstImage@300dpi.ext</span>

If you have a single image, this really does not matter, but if you have hundreds or thousands that need to fit exactly, like in an animation, it becomes important.

#### Position anchor (_-gravity_)

The position set above uses the upper left image corner as reference. In ImageMagick the reference point is called _-gravity_, and upper left is the default. If the text should be in the lower right corner, instead of calculating the image rows and columns from the upper left corner, you can use the lower right corner as the _-gravity_ point, and just give the relative distance from that point. The lower right corner _-gravity_ parameter is _SouthEast_, for other options see the [ImageMagick manual for _-gravity_](http://www.imagemagick.org/script/command-line-options.php#gravity).

<span class='terminal'>$ convert -pointsize 30 -gravity SouthEast -annotate +120+100 "my map" SrcImage.ext  DstImage.ext</span>

The offset distance is always positive towards the image center.

#### Font color

To change the font color when using _-annotate_ add the parameter _-fill_ and the [HEX-code](../2018/2018-01-13-install-imagemagick.html#set-border) for the color you want the text to have:

<span class='terminal'>$ convert -pointsize 30 -fill "#339966" -annotate +20+30 "My map" SrcImage.ext  DstImage.ext</span>

#### Font type

To set the font of any text to _-annotate_, there are several [optional] parameters , _-font_,  _-family_, _-stretch_, _-style_, and _-weight_. ImageMagick can use TrueType fonts (defined in the operating system on your machine), and also other types of fonts. If you stick to the more common fonts this should not be a problem. For setting a font, I will jump straight to an example:

<span class='terminal'>$ convert -font ArialB -family Arial [-stretch Condensed] [-style Italic] [-weight DemiBold] -pointsize 54 -fill "#339966" -annotate +20+30 "My map" SrcImage.ext DstImage.ext</span>

You have to set the _-font_, _-family_ is a backup. _-stretch_, _-style_ and _-weight_ only works for some fonts. To check out the internal ImageMagick fonts and how they can be set use the _-list_ command:

<span class='terminal'>$ magick -list font</span>

### Caption

For my animated time-series, the font size depends on the image size and the length of the time-series, and I do not know it beforehand. I needed some way to rather fit text in a predefined space, than to set the font size. The solution is to define a _-geometry_ and then let ImageMagick fit the text you want to appear into that _-geometry_. Truly Magick for me, as this is exactly what I was looking for. With this solution _-pointsize_ is omitted, but you can still set the other font options outlined above.

<span class='terminal'>$ convert SrcImage.ext -size 200x75 -background none caption:"My map" -geometry +20+30 -composite DstImage.ext</span>

In this function _SrcImage_ is set as the first parameter and then the last function _-composite_ joins the _-geometry_ (of size _-size_) **after** the _-caption_ is set in the _-geometry_.  The text is automatically adjusted to fit in the _-geometry_. To set _-background_ color you give a quoted Hex-code ("#RRGGBB", for example a red background: "#990000"):

<span class='terminal'>$ convert SrcImage.ext -size 200x75! -background "#990000" caption:"My map" -geometry +20+30 -composite DstImage.ext</span>

And with _-font_, _-fill_ (font color) and _-gravity_ added:

<span class='terminal'>$ convert SrcImage.ext -size 200x75! -background "#990000" -font Verdana -fill "#999900" caption:"My map" -gravity SouthEast -geometry +20+30 -composite DstImage.ext</span>

The above example left adjusts the text in the _-geometry_. If you want it centered you need to separate the _-geometry_ from the main image, and use _-label_ instead of _-caption_ (I could not get backslash \\ plus parenthesis "(" to show up, so the starting parenthesis "(" is shown as ")"):

<span class='terminal'> $ convert 300dpi.png \\) -size 200x75 -background "#990000" label:"my map" -trim -gravity center -extent 200x75 \\) -gravity SouthEast -geometry +120+100 -composite DstImage.png</span>

Use _-rotate_ to rotate the text clockwise, inside the separated command for the _-geometry_ (if outside the whole image will be rotated):

<span class='terminal'>$ convert SrcImage.png \\) -size 200x75 -background "#990000" label:"My map" -trim -gravity center -extent 200x75 -rotate "-90" \\) -gravity SouthEast -geometry +120+100 -composite label.png</span>

I got a bit stuck on the above, because I did not keep the space around the ending of the slash+parenthesis "\\)".

## Composite

_-composite_ is the ImageMagick function to combine (overlay, concatenate) two, or more, images. If you folowed the section of setting text above, you will have noted that _-composite_ was used for overlaying a small _-geometry_ over a larger image. The _geometry_ was created as part of the ImageMagick command-line code above, but the principle is the same when combining image files.

  $cmd =" -size 700x400 xc:#262b38  photo10.miff -geometry +2+2 -composite photo40.miff -geometry +410+140 ".
" -composite photo20.miff -geometry +360+20 -composite photo30.miff -geometry +150+130 -composite ";

## Resources
