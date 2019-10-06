---
layout: post
title: Include xml code in Jekyll page
previousurl: null
nexturl: null
categories: "blog"
excerpt: Include complete xml file codes as Hide/Show toggle elements
tags:
  - Jekyll
  - gems
  - update
  - So Simple Theme
image: avg-rntwi_RNTWI_java_2001-2016_AS
date: 2019-06-03 11:33
modified: 2019-09-03 11:33
comments: true
share: true
GRACE-0001_createscaling: GRACE-0001_createscaling
---
<script src="https://karttur.github.io/common/assets/js/karttur/togglediv.js"></script>

## Introduction

Karttur's GeoImagine Framework is run from commands and parameters given in Extensible Markup Language <span class='file'>.xml</span> files. Both for illustrating the structure of the xml files, as well as storing the xml commands in an easily accessible format, I sometimes want to include the complete content of an <span class='file'>.xml</span> file in my posts. But as the xml commands can be very lengthy, I prefer keeping the xml content under a \<div\> element that can [Toggle hide/show code](../blog-hide-show-div/). This post explains how I have solved this for Karttur's GeoImagine blog.

## Jekyll markdown solution

The principal solution for how to include the complete code from an xml file under an element that can toggle between showing and hiding its content, contains 4 parts:

- body markdown code,
- YAML front matter reference,
- yml code under the repo's **\_data** folder expanding the reference, and
- xml converted to html and added to the repo's **\_includes** folder

### Body markdown

To include the complete code of an xml file in a post, add a markdown block like this where you want the xml code (all the back slashes, \\, must be removed, but without them the code is executed):

```
\{\% capture foo \%\}\{\{page.GRACE-0001_createscaling\}\}\{\% endcapture \%\}
\{\% include xml/GRACE-0001_createscaling.html foo=foo \%\}
```

### YAML front matter reference

The YAML front matter links the variable defined in the body to the variable defining the yml variable. They way I ahve built up the solution of Karttur's blogs, they have identical names.

```
GRACE-0001_createscaling: GRACE-0001_createscaling
```

### \_data yml (xml.yml)

You then have to add a variable to the yml file that you use fo.. In my solution I call the yml file <span class=file>yml.xml</span>. Here is the yml code for the xml _GRACE-0001_createscaling_:

```
GRACE-0001_createscaling:
  file: GRACE-0001_createscaling.xml
  html: GRACE-0001_createscaling.html
  id: GRACE-0001_createscaling
  author: Thomas Gumbricht
  caption: null
  credit: null
  source: null
```

### xml converted to html

The html file that Jekyll is going to look for is _GRACE-0001_createscaling.html_. This file must be placed under the <span class='file'>\_includes</span> folder of the repository. I chose to create a subfolder <span class='file'>xml</span> where I then place all my xml converted html file. The html versions also contain the code that effectuates the hide/show toggle, including the Jekyll standard annotation for displaying a code block. I thus do not know how to display the raw html file as a code block including the code for hiding and showing itself as another code block. You just have to open the html file (<span class='file'>GRACE-0001_createscaling.html</span>) directly to see how it can be done.

### The result

Adding the four components outlined above, a button that allows toggling between hide and show is included in your page.

{% capture foo %}{{page.GRACE-0001_createscaling}}{% endcapture %}
{% include xml/GRACE-0001_createscaling.html foo=foo %}

## Creating the components

The four components are cumbersome to create and prone to misspelling, and I create all four using a python package, <span class='package'>xml2html.py</span>. The package is part of Karttur´s GeoImagine Framework, and available in the repository. To run the package requires a source folder (with the xml files) a destination folder (where all the results will end up) and string pattern used for recognising which xml files to to include.

The package produces files associated with each of the four components used for adding xml code to Jekyll pages:

- **copy_and_past_to_markdown.txt** (the code to paste to body),
- **copy_and_past_to_frontmatter.txt** (the code to paste to the YAML frontmatter),
- **copy_and_past_to_xml.yml** (the code to paste to **xml.yml**), and
- Individual html files for each xml (to copy to repo **\_includes** folder)

This solution requires a bit of manual copy and paste, but is much faster than manual editing. And if you update the xml, you only need to change the html file under the repo \_includes folder.
