---
date: '2018-03-30 16:41'
layout: post
title: Setup professional resumé
modified: '2018-03-30 11:24'
categories: blog
excerpt: Customize Jekyll theme for professional resumé
tags:
  - Jekyll
  - Resumé
  - portfolio
  - prefessional
image: ts-mdsl-rntwi_RNTWI_java_2001-2016_AS
comments: true
share: true
---
**Contents**
	\- [Introduction](#introduction)
	\- [Scholar sites and academic social media](#scholar-sites-and-academic-social-media)
	\- [Layout](#layout)
		\- [resume.html](#resumehtml)
		\- [pdfpage.html](#pdfpagehtml)
		\- [pubpage.html](#pubpagehtml)
	\- [\_includes](#includes)
		\- [pdfcontent.html](#pdfcontenthtml)
		\- [simplefooter.html](#simplefooterhtml)
		\- [publication.html](#publicationhtml)
	\- [YAML for pages in the Resumé](#yaml-for-pages-in-the-resum)
	\- [Navigation](#navigation)
		\- [Home and CV](#home-and-cv)
		\- [Projects](#projects)
		\- [Project Example (okavango)](#project-example-okavango)
		\- [Journal articles](#journal-articles)
		\- [Refereed chapters](#refereed-chapters)
		\- [Conference Proceedings](#conference-proceedings)
			\- [Project post (pdfpage.html)](#project-post-pdfpagehtml)
			\- [Project post (pubpage.html)](#project-post-pubpagehtml)
		\- [Publications](#publications)
		\- [Journal articles](#journal-articles)
		\- [Atlas](#atlas)
		\- [Book chapters](#book-chapters)
		\- [Scientific reports](#scientific-reports)
		\- [Conference proceedings](#conference-proceedings)
		\- [Teaching and Talks/Posters](#teaching-and-talksposters)
		\- [Talks](#talks)
		\- [Posters](#posters)
		\- [Supplement](#supplement)
		\- [Blog](#blog)

## Introduction

There are many Jekyll themes developed for use as templates for creating a professional resumé, presentation or portfolio. After having tried a few I felt that the effort was not worth it, and decided to customize the So Simple Theme, the theme I use for my other blogs. This approach was inspired by another resumé template also using So Simple,
[academicpages](https://github.com/academicpages/academicpages.github.io). But I found academicpages over-complex, and created a simpler solution, described in this post.

## Scholar sites and academic social media

As I have worked in academia and international development, my resumé will center on publications and projects. I already have some of my publications and projects listed on social media sites, and the first step was to update my already existing profile pages, and link them to my professional blog.

[Google scholar](https://scholar.google.se/) provides indexing of scholarly literature. As a scholar you can sign up and grab control of your own page, add a picture and a link to some other url.

There are many other sites offering [academic databases and search engines](https://en.wikipedia.org/wiki/List_of_academic_databases_and_search_engines). Some of which are open, some require that you sign up, and some are only accessible by paying subscriptions fees. The largest database is [Scopus](https://www.scopus.com), it is owned by the publisher Elsevier and you have to subscribe to get full access. But as I sometimes do review tasks for Elsevier journals, I get access now and then.

The largest social media site for scholars is [ReasearchGate](https://www.researchgate.net), it is similar to the professional social media site [linkedin](https://www.linkedin.com/).

For my professional pages I chose to include the four sites listed above (Google scholar, Scopus, Researchgate and Linkedin). I wanted them to be in the left column of my professional pages, and I thus edited <span class='file'>\_includes/social-share.html</span> to point towards my profile pages on these sites:

```
{% raw %}
<span class="social-share-googleplus">
  <a href="https://scholar.google.se/citations?user=v9-kWvcAAAAJ&hl=sv" title="Google scholar profile"> Google scholar</a>
</span>
<span class="social-share-googleplus">
  <a href="https://www.researchgate.net/profile/Thomas_Gumbricht" title="Researchgate profile"> Researchgate</a>
</span>
<span class="social-share-googleplus">
  <a href="https://www.scopus.com/authid/detail.uri?authorId=6603689273" title="Scopus profile"> Scopus</a>
</span>
<span class="social-share-googleplus">
  <a href="https://www.linkedin.com/in/thomas-gumbricht-588a2a23/" title="Linkedin profile"> Linkedin</a>
</span>
{% endraw %}
```

## Layout

A [previous post](../so-simple-edits/) describes how to create a new layout. For the professional resumé blog, I created three new layouts, one including the links to my profiles on scholar sites (<span class='file'>resume.html</span>), and two without these links. The latter two are intended for presenting published articles, images, videos, presentations, animations, posters etc, and make use of the full page width. Most of this material is available as pdf files, and rather than converting the pdf to html, I chose to keep the pdf format. Also because modern web-browsers can access and display pdf files effectively. And then the pdf files can also be dowloaded and printed. I thus created a special layout for pdf files <span class='file'>pdfpage.html</span>, and one layout for all other kinds of publication material (text, images, videos, animations etc.), <span class='file'>pubpages.html</span>.

### resume.html

```
{% raw %}
<!doctype html>
<!--[if lt IE 7]><html class="no-js lt-ie9 lt-ie8 lt-ie7" lang="en"> <![endif]-->
<!--[if (IE 7)&!(IEMobile)]><html class="no-js lt-ie9 lt-ie8" lang="en"><![endif]-->
<!--[if (IE 8)&!(IEMobile)]><html class="no-js lt-ie9" lang="en"><![endif]-->
<!--[if gt IE 8]><!--> <html class="no-js" lang="en"><!--<![endif]-->
<head>
{% include head.html %}
</head>

<body id="post">

{% include navigation.html %}

<div id="main" role="main">
  <article class="hentry">
    <div class="entry-wrapper">
      <header class="entry-header">
        <ul class="entry-tags">
          {% for tag in page.tags %}<li><a href="{{ site.url }}/tags/#{{ tag }}" title="Pages tagged {{ tag }}">{{ tag }}</a></li>{% endfor %}
        </ul>

      </header>

      <footer class="entry-meta">
        {% if page.author %}
          {% assign author = site.data.authors[page.author] %}{% else %}{% assign author = site.owner %}
        {% endif %}
        {% if author.avatar contains 'http' %}
          <img src="{{ author.avatar }}" class="bio-photo" alt="{{ author.name }} bio photo"/>
        {% elsif author.avatar %}
          <img src="{{ site.commonurl }}/images/{{ author.avatar }}" class="bio-photo" alt="{{ author.name }} bio photo"/>
        {% endif %}
        <span class="author vcard">By <span class="fn">{{ author.name }}</span></span>

        {% if page.share %}
          {% include social-share.html %}
        {% endif %}
        {% if page.ads == true %}{% include ad-sidebar.html %}<!-- /.google-ads -->{% endif %}
      </footer>
      <div class="entry-content">
        {{ content }}
      </div><!-- /.entry-content -->
    </div><!-- /.entry-wrapper -->

  </article>
</div><!-- /#main -->

<div class="footer-wrapper">
  <footer role="contentinfo" class="entry-wrapper">
    {% include simplefooter.html %}
  </footer>
</div><!-- /.footer-wrapper -->

{% include scripts.html %}

</body>
</html>
{% endraw %}
```

### pdfpage.html

```
{% raw %}
<!doctype html>
<!--[if lt IE 7]><html class="no-js lt-ie9 lt-ie8 lt-ie7" lang="en"> <![endif]-->
<!--[if (IE 7)&!(IEMobile)]><html class="no-js lt-ie9 lt-ie8" lang="en"><![endif]-->
<!--[if (IE 8)&!(IEMobile)]><html class="no-js lt-ie9" lang="en"><![endif]-->
<!--[if gt IE 8]><!--> <html class="no-js" lang="en"><!--<![endif]-->
<head>
{% include head.html %}
</head>
<body id="page">
{% include navigation.html %}
<div id="main" role="main">
  <article class="entry">
    <div class="entry-wrapper">
      <header class="entry-header">
        <h5>
          {% if page.project %} {{ page.project }} <br> {% endif %}
          {% if page.source %}{{ page.source }}{% endif %}
        </h5>
      </header>
      <div class="entry-content">
        {% include pdfcontent.html %}
        {{ content }}
      </div><!-- /.entry-content -->
    </div><!-- /.entry-wrapper -->
  </article>
</div><!-- /#main -->
<div class="footer-wrapper">
  <footer role="contentinfo" class="entry-wrapper">
    {% include simplefooter.html %}
  </footer>
</div><!-- /.footer-wrapper -->
{% include scripts.html %}
</body>
</html>
{% endraw %}
```

### pubpage.html

```
{% raw %}
<!doctype html>
<!--[if lt IE 7]><html class="no-js lt-ie9 lt-ie8 lt-ie7" lang="en"> <![endif]-->
<!--[if (IE 7)&!(IEMobile)]><html class="no-js lt-ie9 lt-ie8" lang="en"><![endif]-->
<!--[if (IE 8)&!(IEMobile)]><html class="no-js lt-ie9" lang="en"><![endif]-->
<!--[if gt IE 8]><!--> <html class="no-js" lang="en"><!--<![endif]-->
<head>
{% include head.html %}
</head>

<body id="page">

{% include navigation.html %}

<div id="main" role="main">
  <article class="entry">

    <div class="entry-wrapper">
      <header class="entry-header">
        <h4>{% if page.source %}{{ page.source }}{% endif %}</h4>
      </header>
      <div class="entry-content">
        {{ content }}
      </div><!-- /.entry-content -->
    </div><!-- /.entry-wrapper -->
  </article>
</div><!-- /#main -->

<div class="footer-wrapper">
  <footer role="contentinfo" class="entry-wrapper">
    {% include simplefooter.html %}
  </footer>
</div><!-- /.footer-wrapper -->

{% include scripts.html %}

</body>
</html>
{% endraw %}
```

## \_includes

The new layouts introduce new Jekyll liquids calling <span class='file'>\_includes/pdfcontent.html</span> and <span class='file'>/\_includes/simplefooter.html</span>, and then I also added <span class='file'>\_includes/publication.html</span> that I use for adding publications belonging to different categories and projects. It will become clear later.

### pdfcontent.html

```
{% raw %}
<script>
  function setIframeSrc() {
    document.getElementById("pdfFrame").src = "http://docs.google.com/gview?url={{ site.commonurl }}/pdf/{{ page.pdf }}&embedded=true";
  }
</script
<a></a>
<button onclick="setIframeSrc()"><span
 style="text-decoration:underline;">Load pdf in frame</span></button> or
<a style="text-decoration:underline;" href="{{ site.commonurl }}/pdf/{{ page.pdf }}">
  go to the pdf
</a> (or right click the latter and 'Save Link As ...').
<figure>
  <iframe id="pdfFrame"
    style="width:720px; height:576px;" frameborder="1">
  </iframe>
</figure>
{% endraw %}
```

### simplefooter.html

<span class='file'>\_includes/simplefooter.html</span> is a copy of <span class='file'>\_includes/footer.html</span>, but only keeping the social media links.


### publication.html

```
{% raw %}
<li>
<article>
<span style="font-size: 80%; display: block;">
  {{ post.authors | remove: '\[ ... \]' | remove: '\( ... \)' | markdownify | strip_html | strip_newlines | escape_once }},
{% if post.doiurl %}
  {% if post.doiurl contains 'http' %}
    <a href="{{ post.doiurl }}">
  {% else %}
    <a href="{{ site.url }}{{ post.url }}">
  {% endif %}
    {{ post.source }}
    <span style="font-weight: bold;"><time datetime="{{ post.date |  date_to_xmlschema }}">{{ post.date | date: "%Y" }}</time></span>.
    {% if post.access %}
      {% if post.access == 'open' %}
        <img src = "{{ site.commonurl }}/images/openaccess.png" style="width:30px;height:20px;border:0;"  alt='open'>
      {% elsif post.access == 'researchgate' %}
          <img src = "{{ site.commonurl }}/images/rgaccess.png" style="width:20px;height:20px;border:0;"  alt='researchgate'>
      {% elsif post.access == 'pdfaccess' %}
        <img src = "{{ site.commonurl }}/images/pdfaccess.png" style="width:48px;height:20px;border:0;" alt='karttur'>
        {% elsif post.access == 'movieaccess' %}
          <img src = "{{ site.commonurl }}/images/movieaccess.png" style="width:72px;height:20px;border:0;" alt='movie'>

      {% elsif post.access == 'closed' %}
        <img src = "{{ site.commonurl }}/images/closedaccess.png" style="width:30px;height:20px;border:0;" alt='closed'>
      {% endif %}
    {% endif %}
    </a>
{% else %}
    {{ post.source }}
    <a href="#">
    <span style="font-weight: bold;"><time datetime="{{ post.date |  date_to_xmlschema }}">{{ post.date | date: "%Y" }}</time></span>.
    </a>
{% endif %}
</span>
<a href="{{ site.url }}{{ post.url }}">{{ post.title }}</a>
</article>
</li>
{% endraw %}
```

## YAML for pages in the Resumé

The different layouts and \_includes expect some additional page YAML parameters, and this is the complete YAML for posts in the professional blog:

```
{% raw %}
---
title: "Title"
authors: "NN"
layout: resume/pubpage/pdfpage
categories: project/projectid
source: 'publication source'
projectid:
pattern: 'noun' #like a tag
process: 'verb' #like a tag
pages:
number:
issue:
editor:
date: 1988-01-01
enddate: 1993-12-31 #for projects only
projurl: ../projectid/ #for projects only
summary: '' #for projects only
tag:
  - tag1
  - tag2
---
{% endraw %}
```

All YAML parameters are not needed for all types of posts.

## Navigation

For my professional resumé I chose the following navigation entries:

- Home
- CV
- Projects
- Publications
- Teaching
- Talks/posters
- Supplement
- Blog

Home is the front page of the resumé, CV the Curriculum Vitae, under Projects I mix my research projects, courses I have been teaching and also different positions I have held at different organizations. Under Publications I list all articles, books and chapters in books that I have written. Teaching contains lectures and different training materials. Talks/posters contain pdf versions of presentations and images of posters that I have presented. Supplement contains bits and pieces that does not fit anywhere else, like animations and background materials. Blog links to my other blogs on Github.

<span class='file'>navigation.yml</span> then looks like this for my resumé blog.
```
{% raw %}
- title: "Home"
  url: /
- title: "CV"
  url: /cv/
- title: "Projects"
  url: /projects/
- title: "Publications"
  url: /publications/
- title: "Teaching"
  url: /teaching/
- title: "Talks/posters"
  url: /talks/
- title: "Supplement"
  url: /supplement/
- title: "Blog"
  url: /blog/
  {% endraw %}
```

All the url links in the navigation must be created, along with an <span class='file'>index.md</span> file that captures the content intended for that category. It will be elaborated later, but you might want to create the sub-folders now that you see them listed together.

In the following sections I use navigation entries to explain how the resumé blog is built up with its different posts, categories, layouts and includes.

### Home and CV

The [home (front) page of the resumé](https://karttur.github.io/professional/) and the [CV page](https://karttur.github.io/professional/cv/) use the resume layout (scholar profile links in left column) but then only contain ordinary markdown code, with no calls to any \_include and no posts included.

Home is changed to markdown file <span class='file'>index.md</span> in the root of the repository. Also CV is a markdown file, but under <span class='file'>cv/index.md</span>.

### Projects

The [project page](https://karttur.github.io/professional/projects/), <span class='file'>projects/index.md</span>:
```
{% raw %}
---
layout: resume
title: Projects and tasks
excerpt: "Archive of projects"
search_omit: true
share: true
---

<ul class="post-list">
  {% for post in site.categories.project %}
    {% if post.projurl contains 'http' %}
      {% assign domain = '' %}
    {% else %}
      {% assign domain = post.projurl %}
    {% endif %}
    <li><article>

    <a href="{{ domain }}{{ link.url }}">{{ post.title }}</a>

    (<span style="font-weight: bold;"><time datetime="{{ post.date | date_to_xmlschema }}">
    {{ post.date | date: "%Y" }}</time> - <time datetime="{{ post.enddate | date_to_xmlschema }}">{{ post.enddate | date: "%Y" }}</time></span>).

    {% if post.summary %}
      <span style="font-size: 80%; display: block;">{{ post.summary | remove: '\[ ... \]' | remove: '\( ... \)' | markdownify | strip_html | strip_newlines | escape_once }}
      </span>
    {% endif %}

    </article></li>
  {% endfor %}
</ul>

{% endraw %}
```

The project main page lists all posts of the category "project", including title, start year and end year, and summary. A project is thus created by entering a post, and setting the category to project in the post YAML:

```
{% raw %}
---
title: "Okavango post-doc studies (still continuing)."
layout: resume
categories: project
source:
date: 1999-10-01
enddate: 2018-03-20
projectid: okavango
summary: "1999 I got a post-doc scholarship from the Royal Academy of Sciences in Sweden, allowing me to move to South Africa and start a post-doc at University of the Witwatersrand (Wits) in Johannesburg. My studies focused on the Okavango swamps in Botswana, but also other regional wetlands. I stayed at Wits for two years, and also worked for a few months with the tourist industry in the Okavango."
projurl: "../okavango/"
---
{% endraw %}
```

The project post contains nothing but the YAML. But it expects to find a subfolder (e.g. "../okavango/" in the example), and you must also add a markdown page that captures any post related to the projectid ("okavango"). Link to the live [project page](https://karttur.github.io/professional/projects/) in my resumé.

### Project Example (okavango)

The post above creates the project "okavango", and you must now also create a folder directly under the root of your repository called <span class='file'>okavango</span>. Create a markdown file <span class='file'>okavango/index.md</span>, it should be of the category resume, and with a title and excerpt.

```
{% raw %}
---
layout: resume
title: Okavango post-doc studies (still continuing).
excerpt: "An archive of Okavango publications"
search_omit: true
share: true
---
1999 I got a post-doc scholarship from the Royal Academy of Sciences in Sweden, allowing me to move to South Africa and start a post-doc at University of the Witwatersrand (Wits) in Johannesburg. My studies focused on the Okavango swamps in Botswana, but also other regional wetlands. I stayed at Wits for two years, and also worked for a few months with the tourist industry in the Okavango.

I have since then used my data and knowledge about the Okavango in many other studies, including my efforts in mapping and monitoring global tropical wetlands and peatlands.

### Journal articles

<ul class="post-list">
{% for post in site.categories.journal %}
  {% if post.projectid == "okavango" %}
    {% include publication.html post=post %}
  {% endif %}
{% endfor %}  
</ul>

### Refereed chapters

<ul class="post-list">
{% for post in site.categories.refereechapter %}
  {% if post.projectid == "okavango" %}
    {% include publication.html %}
  {% endif %}
{% endfor %}
</ul>

### Conference Proceedings

<ul class="post-list">
{% for post in site.categories.conference %}
  {% if post.projectid == "okavango" %}
    {% include publication.html post=post %}
  {% endif %}
{% endfor %}  
</ul>

{% endraw %}
```

The project markdown file (<span class='file'>okavango/index.md</span>) loops over all the posts in the blog, and first looks for different categories ("journal", "refereechapter" and "conference" in the example), and then includes the post if the category is correct and also the projectid is correct ("okavango" in the example). All identified posts are included through the liquid calling the \_include publication.html. Link to the live [Okavango project](https://karttur.github.io/professional/okavango/) in my professional pages.

Let us check two posts, of which the first belongs to the  "okavango" project.

#### Project post (pdfpage.html)

If the post to add to a project consists of a pdf file (e.g. a published manuscript, a presentation etc), then I use the layout <span class='file'>pdfpage.html</span>, and only the YAML is needed to create the post.

```
{% raw %}
---
title: "Portraying the geophysiology of the Okavango Delta, Botswana"
authors: "Gumbricht, T., McCarthy, J, & McCarthy, T."
layout: pdfpage
categories: talk
source: '<i>28th Intl Symp on Remote Sensing of Environment</i>, 2000, Cape Town, South Africa'
pdf: oka_RSENV_capetown_2000_tg.pdf
doiurl: '#'
date: 2000-06-01
projectid: okavango
project: Okavango
pattern:
process:
pages:
number:
issue:
editor:
access: 'pdfaccess'
---
{% endraw %}
```

[Link to this post in the professional pages](https://karttur.github.io/professional/conference/conf-oka-geophysiology/)

#### Project post (pubpage.html)

All posts related to a particluar project but not with a pdf file at the core, must be created with the layout <span class='file'>pubpage.html</span>. The code to write is just ordinary markdown, and depend on the material that is presented. The example below shows how to present a poster as a jpg file.

```
{% raw %}
---
title: "Interated management of Lake Kyoga natural resources."
authors: "Gumbricht, T."
layout: pubpage
categories: poster
source: '<i>Department of Water Development</i>, 2004, Entebbe, Uganda'
figure1: kyoga_images-poster_entebbe_20040401_quicklook
doiurl: '#'
date: 2004-04-01
projectid: kyoga
project: Integrated management of Lake Kyoga natural resources
access: 'imageaccess'
---

<figure>
<figcaption>Slide the mouse over the image, right click and select "Save Link As" or click on the map to get a pop-up image that you can drag to your desktop. The full size image is a jpg file in A0 format (4.1 MB).</figcaption>

<a href="{{ site.commonurl }}/images/{{ site.data.images[page.figure1].source }}"><img src="{{ site.commonurl }}/images/{{ site.data.images[page.figure1].file }}" alt="image"></a>
</figure>

{% endraw %}
```

The poster image, both the smaller sized image shown in the post, and the larger (full) poster image are included using the image solution outlined in [this post on common assets](../common-assets/). The page with [poster in the live professional site](https://karttur.github.io/professional/poster/poster-kyoga-sat-images/).

### Publications

The navigation tab for Publications links to a page (<span class='file'>publications/index.md</span>) listing all publications, divided by categories.

```
{% raw %}
---
layout: resume
title: Publications
excerpt: "An archive of articles sorted by publication type and date."
search_omit: true
share: true
---
Click on source icon to get online pdf versions (articles with no icon are not available online), or click the title to see abstract.

### Journal articles

<ul class="post-list">
{% for post in site.categories.journal %}
    {% include publication.html post=post %}
{% endfor %}  
</ul>

### Atlas

<ul class="post-list">
{% for post in site.categories.atlas %}
    {% include publication.html post=post %}
{% endfor %}  
</ul>

### Book chapters

<ul class="post-list">
{% for post in site.categories.refereechapter %}
    {% include publication.html post=post %}
{% endfor %}  
</ul>

### Scientific reports

<ul class="post-list">
{% for post in site.categories.report %}
    {% include publication.html post=post %}
{% endfor %}  
</ul>

### Conference proceedings

<ul class="post-list">
{% for post in site.categories.conference %}
    {% include publication.html post=post %}
{% endfor %}  
</ul>

{% endraw %}
```
As each publication is in a separate post, there will be an html page for each publication, created as explained in the previous section.

### Teaching and Talks/Posters

The pages for Teaching and Talks/Posters follow the same principles as the Publications page. The only thing that differs are the post categories that are included under each entry. The teaching categories could for instance include, lectures, training material, group work, assignments etc. It really depends on the kind of teaching/training you have been involved with. Talks and posters seem easier to categorise:

```
{% raw %}

---
layout: resume
title: Talks and Poster presentations
excerpt: "Archive of talks and poster presentations"
search_omit: true
share: true

---

All presentations are available as pdf files (talks) or jgp images (posters). Click on the title to get to the page where you can read or download the source file.

### Talks

<ul class="post-list">
{% for post in site.categories.talk %}
  {% include publication.html post=post %}    
{% endfor %}
</ul>

### Posters

<ul class="post-list">
{% for post in site.categories.poster %}
  {% include publication.html post=post %}    
{% endfor %}
</ul>

{% endraw %}
```

### Supplement

Under supplements I have collected bits and pieces that really do not fit under the other headings.

```
{% raw %}
---
layout: resume
title: Supplements
excerpt: "An archive of supplements."
search_omit: true
share: true
---

Under supplements I have collected bits and pieces that really do not fit under the other headings.


<ul class="post-list">
{% for post in site.categories.supplement %}
  {% include publication.html post=post %}
{% endfor %}
</ul>

{% endraw %}
```

### Blog

The blog page links to my other blogs, there are no posts linked to the blog page. The file <span class='file'>blog/index.md</span> is just compoed of ordinary markdown code with no liquids.
