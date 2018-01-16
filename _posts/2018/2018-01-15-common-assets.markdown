---
layout: post
title: Common assets across sites
modified: "2018-01-15 22:33"
categories: blog
excerpt: Creating common assets across sites with the So Simple Theme.
tags:
  - Jekyll  
  - assets
  - images
  - So Simple Theme
image: avg-rntwi_RNTWI_java_2001-2016_AS
date: "2018-01-15 20:34"
comments: true
share: true
---
**Contents**
	\- [introduction](#introduction)
	\- [Jekyll assets](#jekyll-assets)
	\- [Images](#images)
		\- [A side note on Image dimensions](#a-side-note-on-image-dimensions)
		\- [images.yml](#imagesyml)
		\- [Linking images.yml to the page](#linking-imagesyml-to-the-page)
	\- [\_config.yml](#configyml)
	\- [Other Images](#other-images)
	\- [Common CSS and JavaScript](#common-css-and-javascript)

## introduction

As I chose to publish what I do in several thematic blog sites, rather than a single blog site with everything mixed, I needed to create a solution where I could use a common set of assets (javascript, css, fonts, images and other resources) across all sites. This post is about how I did it.

## Jekyll assets

When setting up a Jekyll site, the assets used by the site are typically assembled in a folder, usually calls <span class ='file'>assets</span>. Assets can either be hardcoded (explicitly stated), or created by Jekyll when serving up. In the latter case, Jekyll assembles the codes that are stored under the folder <span class='file'>\_sass</span> and incorporates them under <span class='file'>assets</span> in the published site (under the folder <span class='file'>\_site</span>). The theme I use for Karttur´s blog, So Simple, uses this approach.

As I edited and expanded my resources, I got difficulties in keeping track of my <span class='file'>\_sass</span> contents for different sites within my GitHub pages, and I decided to build a repository with all assets and other resources that are common across my different sites (repositories). This also avoids having duplicate codes and images in my repositories.

## Images

Most individual Karttur web-pages include a map at the top. I wanted to simplify the handling of images by creating a single repository for all images, including maps.

All maps that are shown on Karttur´s GitHub pages are generated from Karttur´s Geo Imagine Framework. I thus added a small script that automatically saves all maps as images in a hierarchy of folders. I then assemble all maps covering the same region, and are at the same resolution in separate folder. All maps in the latter thus have the same dimension (the number of columns and rows are the same). This is not necessary to do, but that is how did it. This post is merely about how to asemble all images in a separate repository.

### A side note on image dimensions

The maps on top of Karttur's pages all have a width (columns) of 1900 pixels, and then I allow the height (rows) to vary between approximately 400 and 500 pixels. None of my maps have these dimensions when they are produced from Karttur´s Geo Imagine Framework. Selecting which section (geometry) of each map to use as an image in my pages must be done manually. The processing required to massage the map into the required dimensions is done with [ImageMagick](../install-imagemagick/). Typically I combine the ImageMagick functions _-resize_, and _-crop_ to get the map parts I want, and then I add the _-border_ and compress the image to a quality of 70 to 80 %. The example below shows how I created the map of Java (on top of this page) from a map of Indonesia. Using [ImageMagick](../install-imagemagick/) <span class='app'>Terminal</span> command-line syntax, as explained in [another post](../add-text-to-image), the command-line syntax becomes:

```
# The lines starting with '#' are comments added for this post
# *.tif - All images from Karttur´s Geo Imagine Framework are tif files
# -resize "3600>": to reduce the dimensions, otherwise Java is too wide to fit in 1900 pixels
# -gravity  As Java is in the south of Indonesia I use -gravity south
# -crop 1896x396-150+10: cuts out JavaScript
# -border 2x2: adds a border of 2 pixels around the image, and the image thus becomes 1900x400 pixels
# -bordercolor "#000000": sets border color to black
# -quality 80: sets the image quality

for i in *.tif; do convert -resize "3600>" -gravity south -crop 1896x396-150+10 -border 2x2 -bordercolor "#000000" -quality 80 "$i" "pub-images/${i%.*}.jpg"; done

# change directory (cd) to pub-images

cd pub-images

# Rename the produced image by replacing 'id' with 'java' (all country maps are coded to the ISO 2-letter code in Karttur's Geo Imagine Framework.)

for f in *.jpg; do mv -v "$f" "${f/_id_/_java_}"; done;

# change directory (cd) back to the parent folder

cd ..
```
I run the above code as a shell script. This means I put the code above in a file with the extension ".sh", like <span class='file'>indonesia_images.sh</span> and put that file together with my image maps over Indonesia. I then have to make <span class='file'>indonesia_images.sh</span> an executable file by changing its permission at the <span class='app'>Terminal</span> prompt:

<span class='terminal'>$ chmod 777 indonesia_images.sh</span>

And then run it by typing its name at the <span class='app'>Terminal</span> prompt:

<span class='terminal'>$ ./indonesia_images.sh</span>

The prefix <span class='terminal'>./</span> is required for executable files in my (macOS) operating system.

Whenever I add a new map of Indonesia to the same folder, I just rerun the shell (<span class='app'>Terminal</span>) script again.

### images.yml

With all the map images set to the correct dimension, I have a Python script that generates an yml file <span class='file'>images.yml</span>, with the following structure for each image:

```
avg-rntwi_RNTWI_java_2001-2016_AS:
    file: avg-rntwi_RNTWI_java_2001-2016_AS.jpg
    author: Thomas Gumbricht
    caption: Average rain normalized soil moisture 2001-2016, Java, Indonesia
    credit: null
    source: null
```

The example above shows the yml entry for the map shown at the top of this page.

### Linking images.yml to the page

With the images and their metadata given in <span class='file'>images.yml</span> the image entry in the page YAML can be simplified to:

```
image: avg-rntwi_RNTWI_java_2001-2016_AS
```

And then I connect the image given in the page YAML, by identifying the liquid (this is a liquid: \{\{\}\}) in the markdown (or html) code that defines the image and its source. For the So Simple theme that is in the <span class='file'>\_layout/post.html</span> and <span class='file'>\_layout/page.html</span>. I change the code to capture the metadata as I entered it in <span class='file'>images.yml</span>:


```
{% raw %}
#This is how it looks after editing
{% if page.image %}
  {% assign image = site.data.images[page.image] %}
  <img src="{{ site.commonurl }}/images/{{ image.file }}" class="entry-feature-image" alt="{{ image.caption }}" {% if site.logo == null %}style="margin-top:0;"{% endif %}>{% if image.caption %}<p class="image-credit">Map: {{ image.caption }}</p>{% endif %}
{% endif %}
{% endraw %}
```

The first line
```
{% raw %}
# First line
{% if page.image %}
{% endraw %}
```
checks if the page YAML defines an image (as above "image: avg-rntwi_RNTWI_java_2001-2016_AS") or is set to "null" (no image).

The second line
```
{% raw %}
# Second line
{% assign image = site.data.images[page.image] %}
{% endraw %}
```
looks in <span class='file'>\_data/images.yml</span> (denoted as "site.data.image" in the code) and assigns the entry with the name "page.image" (the image defined in the page YAML, "avg-rntwi_RNTWI_java_2001-2016_AS" in the example above) to the variable "image".

The third line
```
# Third line
{% raw %}
<img src="{{ site.commonurl }}/images/{{ image.file }}" class="entry-feature-image" alt="{{ image.caption }}" {% if site.logo == null %}style="margin-top:0;"{% endif %}>{% if image.caption %}<p class="image-credit">Map: {{ image.caption }}</p>{% endif %}
{% endraw %}
```
defines the url link to the image, and sets the caption text to be shown under the image (it also handles the logo in the So Simple Theme that I use). The image file name is in a child variable to the assigned image variable "image.file", as defined in <span class='file'>\_data/images.yml</span>. And the caption is in another child "image.caption". The first liquid defines a new variable "site.commonurl". The parent "site" refers to the site <span class='file'>\_config.yml</span> file, and the variable "commonurl" must be defined in <span class='file'>\_config.yml</span>.

## \_config.yml

For the new image library solution to work, <span class='file'>\_config.yml</span> must be updates with a link to the GitHub repository with the common resources ( where you must include the <span class='file'>images</span> folder). In my solution I call this variable "commonurl", and thus I added the following line to my <span class='file'>\_config.yml</span>:
```
{% raw %}
commonurl: https://karttur.github.io/common
{% endraw %}
```
The variable "commonurl" links to a GitHub repository with a "gh-pages" branch, that contains the common resources (including images) that are used by all Karttur's GitHub pages built on the So Simple Theme, except this blog (on building a theme blog in Jekyll); in order to keep it as a stand-alone solution for cloning or downloading on GitHub this blog does not link to Karttur´s "common" repository.

### Other Images

I also put other images used in Karttur´s pages in the repository "common". Subsequently I then also changed the link in all liquids linking images from \{\{site.url\}\} to \{\{site.commonurl\}\}.

## Common JavaScripts, CSS and fonts

Javascripts (files with the extension .js) used in Jekyll themes are in general found assembled together under a resource folder, typically under the path <span class='file'>assets/js/</span> or similar. Moving the folder with javascripts to another repository is straight forward. You only need to change all the internal links to the javascripts to the repository (site) where you put them up. For the So Simple Theme that I use, the reference to javascripts are given using a liquid \{\{ site.url \}\}, and I changed that to \{\{ site.commonurl \}\}, and gave the link to the repository where I put them up in <span class='file'>\_config.yml</span> as [outlined above](#configyml). There were

If your Jekyll Theme contains <span class='file'>.scss</span> files, the CSS styles in those files are transferred to the <span class='file'>\_site</span> (the published web page) by Jekyll when you tell Jekyll to serve. To use a common CSS resource for multiple sites you must copy the CSS file from under the <span class='file'>\_site</span> folder to your site that holds the common files. The name and path to the CSS resources should replicate the name and path in the <span class='file'>\_site</span>.

I solved this by keeping the <span class='file'>.scss</span> files for one of my repositories, and then use that site to infer changes to my CSS, and then push those changes to the GitHub repository "common". For all other repositories I deleted the <span class='file'>.scss</span> files.

The So Simple theme also includes font assets (<span class='file'>assets/fonts/</span> files), and I also moved these to the "common" repository.

I thus moved the complete set of <span class='file'>assets</span> to the special repository "common". After changing all the links in the <span class='file'>.html</span> and <span class='file'>.markdown</span> files in my Theme, I could remove the assets from the repositories for my different theme sites. I could also remove all the <span class='file'>.scss</span> files, and all images. The repositories for the theme blogs became cleaner, and actually serve up double as fast. With the "common" repository up and running, there is no difference in the pages I serve up with the <span class='app'>Terminal</span> using Jekyll to create the localhost server.

<span class='terminal'>bundle exec jekyll serve</span>

The pages in the site looks exaclty like before I created the "common" repository and reset all the links to javasctripts, CSS, fonts and images.

## Is it worth it?

It took me some time to alter my pages to have common resources for images, javascript, css and fonts. But having done it, it is much easier to start a new blog theme in a separate GitHub repository. And I can change the style (CSS), behaviour (javascript) and fonts by just making changes in one place.

The largest advantage for me, however, was to assemble all image maps in a single repository, and then just be able to include images by adding the a single line that then accesses both the image and the image meatadata via <span class='file'>images.yml</span>.

IMAGES.YML IN DATA


## Resources

[Karttur´s "common" repository](#)
