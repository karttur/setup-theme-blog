---
layout: post
title: Jekyll theme customization
previousurl: null
nexturl: null
excerpt: Costumizing a Jekyll theme
modified: '2018-01-20 13:33'
categories: blog
tags:
  - Jekyll theme customization
  - So Simple Theme
  - YAML edit
  - assets
image: ts-upsl-rntwi_RNTWI_id_2001-2016_AS
date: '2018-01-20 14:05'
comments: true
share: true
---
**Contents**
	\- [Introduction](#introduction)
	\- [Navigation (top menu)](#navigation-top-menu)
		\- ['site-articles' index.md](#site-articles-indexmd)
		\- [layout (navigation links)](#layout-navigation-links)
		\- [Includes (footer)](#includes-footer)
		\- [Javascript](#javascript)
		\- [CSS](#css)
		\- [Common assets and images](#common-assets-and-images)
		\- [Front matter (\_config.yml and YAML)](#front-matter-configyml-and-yaml)

##Introduction

This post summarizes the the customizations done to the Jekyll So Simple theme for the Karttur GitHub sites based on So Simple.  

## Navigation (top menu)

Many Jekyll template themes include a menu system, that when cloned or downloaded is setup to give access to the Theme instructions. In most cases you need to edit the menu items to fit the needs of your site. For the So Simple Theme, the main menu is defined in <span class='file'>\_data/navigation.yml</span>. For Karttur's pages, I changed <span class='file'>navigation.yml</span> like this:

```
- title: About
  url: /about/
  include: 'N'

- title: Front
  url: /
  include: 'N'

- title: Karttur on GitHub
  url: https:karttur.github.io/overview/
  include: 'N'

- title: Articles
  url: /'site-articles'/
  include: 'N'

- title: Blog
  url: /blog/
  include: 'N'

- title: Search
  url: /search/
  include: 'N'
```

The Karttur pages on GitHub.com are thematically separated in different repositories (blogs).
To keep track of the different repositories I chose to give different names to the 'article' class of
each repository. Thus the line "url: /'site-articles'/" varies for each repository, and the change must
then be reflected in the file generating the 'articles', which for the Karttur solution translates to creating a new repository folder, with an adjusted <span class='file>index.md</span> file </span class='file>'site-articles'/index.md</span>.

### 'site-articles' index.md

As Karttur's blogs cover different thematic areas, for any new theme I rename  the general 'article' folder to a name reflecting the theme, and edit the <span class='file>index.md</span> file in that renamed folder. As an example, in the 'overview' blog, 'article' is replace with 'overview',
and then </span class='file>'overview/index.md</span> is edited to look for posts with the YAML parameter _categories_ set to 'overview':

```
<ul class="post-list">
{% for post in site.categories.overview %}
  <li><article><a href="{{ site.url }}{{ post.url }}">{{ post.title }} <span class="entry-date"><time datetime="{{ post.date | date_to_xmlschema }}">{{ post.date | date: "%B %d, %Y" }}</time></span>{% if post.excerpt %} <span class="excerpt">{{ post.excerpt | remove: '\[ ... \]' | remove: '\( ... \)' | markdownify | strip_html | strip_newlines | escape_once }}</span>{% endif %}</a></article></li>
{% endfor %}
</ul>
```
Consequently the YAML for posts that belong to the 'article' class must be edited to state 'overview' for the parameter _categories_ .

```
___
...
categories: overview
...
___
```

### layout (navigation links)

The overall page structure of a Jekyll theme is defined by a layout, usually under the <span class='file>\_layout</span> folder. The So Simple theme contains two layouts, <span class='file>\_layout/page.html</span> and <span class='file>\_layout/post.html</span>.
The style for the layouts are the same, but with the _page_ layout omitting some features compared to _post_. For the Karttur site I wanted to have an intermediate layout between _page_ and _post_, and created (<span class='file>\_layout/article.html</span>). The only difference between the classes _post_ and _article_ is in the navigation linkage to 'previous' and 'next' towards the
bottom, where the _article_ class looks like this:

```
{% raw %}
    <nav class="pagination" role="navigation">
      {% if page.previousurl %}
        <a href="{{ site.url }}/{{ page.categories }}/{{ page.previousurl }}/" class="btn" title="previous">Previous</a>
      {% endif %}
      {% if page.nexturl %}
        <a href="{{ site.url }}/{{ page.categories }}/{{ page.nexturl }}/" class="btn" title="next">Next</a>
      {% endif %}
    </nav><!-- /.pagination -->
{% endraw %}
```

And then I also edited the _post_ class previous' and 'next' navigation:
```
    <nav class="pagination" role="navigation">
      {% if page.previousurl %}
        <a href="{{ site.url }}/{{ page.categories }}/{{ page.previousurl }}/" class="btn" title="previous">Previous</a>
      {% elsif page.previous %}
          <a href="{{ site.url }}{{ page.previous.url }}" class="btn" title="{{ page.previous.title }}">Previous</a>
      {% endif %}

      {% if page.nexturl %}
        <a href="{{ site.url }}/{{ page.categories }}/{{ page.nexturl }}/" class="btn" title="next">Next</a>
      {% elsif page.next %}
        <a href="{{ site.url }}{{ page.next.url }}" class="btn" title="{{ page.next.title }}">Next</a>
      {% endif %}
    </nav><!-- /.pagination -->
```

Compared to the orignal So Simple theme definitions, this gives the option to fully control the navigation links to 'previous' and 'next' for both the layout _posts_ and _articles_,
but also required two new YAML parameters: _previousurl_ and _nexturl_:

```
___
...
previousurl: 'previous-post-name' (or 'null')
nexurl: 'next-post-name' (or 'null')
...
___
```

If set to 'null' these parameters are ignored, and the layout is the same as for the original So Simple theme.

### Includes (footer)

The <span class='file>\_includes/footer.html</span> for Karttur's pages is extended compared to the original So Simple theme and contains the site title and description, and a link to [researchgate](https://researchgate.net) as a social site.

```
{% raw %}
{% if site.owner.google.ad-client and site.owner.google.ad-slot %}{% include ad-footer.html %}{% endif %}
<div class="wrap">
	<h1 class='foot-title'>{{ site.title | escape }}</h1>
	<h2 class='foot-description'>{{ site.description | escape }}</h2>
</div>
<span>&copy; {{ site.time | date: '%Y' }} {{ site.owner.name }}. Powered by <a href="http://jekyllrb.com" rel="nofollow">Jekyll</a> using the <a href="https://mademistakes.com/work/so-simple-jekyll-theme/" rel="nofollow">So Simple Theme</a>.</span>

<div class="social-icons">
	{% if site.owner.twitter %}<a href="https://twitter.com/{{ site.owner.twitter }}" title="{{ site.owner.name}} on Twitter" target="_blank"><i class="fa fa-twitter-square fa-2x"></i></a>{% endif %}
	{% if site.owner.facebook %}<a href="https://facebook.com/{{ site.owner.facebook }}" title="{{ site.owner.name}} on Facebook" target="_blank"><i class="fa fa-facebook-square fa-2x"></i></a>{% endif %}
	{% if site.owner.google.plus %}<a href="https://plus.google.com/+{{ site.owner.google.plus }}" title="{{ site.owner.name}} on Google+" target="_blank"><i class="fa fa-google-plus-square fa-2x"></i></a>{% endif %}
	{% if site.owner.linkedin %}<a href="https://linkedin.com/{{ site.owner.linkedin }}" title="{{ site.owner.name}} on LinkedIn" target="_blank"><i class="fa fa-linkedin-square fa-2x"></i></a>{% endif %}
	{% if site.owner.researchgate %}<a href="https://researchgate.net/{{ site.owner.researchgate }}" title="{{ site.owner.name}} on Researchgate" target="_blank"><i class="fa fa-pinterest fa-2x"></i></a>{% endif %}
	{% if site.owner.stackexchange %}<a href="{{ site.owner.stackexchange }}" title="{{ site.owner.name}} on StackExchange" target="_blank"><i class="fa fa-stack-exchange fa-2x"></i></a>{% endif %}
	{% if site.owner.instagram %}<a href="https://instagram.com/{{ site.owner.instagram }}" title="{{ site.owner.name}} on Instagram" target="_blank"><i class="fa fa-instagram fa-2x"></i></a>{% endif %}
	{% if site.owner.flickr %}<a href="https://www.flickr.com/photos/{{ site.owner.flickr }}" title="{{ site.owner.name}} on Flickr" target="_blank"><i class="fa fa-flickr fa-2x"></i></a>{% endif %}
	{% if site.owner.github %}<a href="https://github.com/{{ site.owner.github }}" title="{{ site.owner.name}} on Github" target="_blank"><i class="fa fa-github-square fa-2x"></i></a>{% endif %}
	{% if site.owner.tumblr %}<a href="http://{{ site.owner.tumblr }}.tumblr.com" title="{{ site.owner.name}} on Tumblr" target="_blank"><i class="fa fa-tumblr-square fa-2x"></i></a>{% endif %}
  {% if site.owner.pinterest %}<a href="https://www.pinterest.com/{{ site.owner.pinterest }}/" title="{{ site.owner.name}} on Pinterest" target="_blank"><i class="fa fa-pinterest fa-2x"></i></a>{% endif %}
	{% if site.owner.weibo %}<a href="https://www.weibo.com/u/{{ site.owner.weibo }}/" title="{{ site.owner.name}} on Weibo" target="_blank"><i class="fa fa-weibo fa-2x"></i></a>{% endif %}
  <a href="{{ site.url }}/feed.xml" title="Atom/RSS feed"><i class="fa fa-rss-square fa-2x"></i></a>
</div><!-- /.social-icons -->
{% endraw %}
```

### Javascript

To include large chunks of code in the posts, but hiding it as default and only showing it on request, I added a javascript function described in a [separate post](../../setup-blog/blog-hide-show-div/). The use of the hide/show function is illustrated in the next section on CSS.

### CSS

I customized the heading font size for \<h1\> and \<h\2>, by reducing them.

I then added CSS for the \<span\> tags that are used throughout the Karttur site to fasciliate the explanations regarding which apps, structures and commands etc that the text refers to.

<button id= "toggle-codelbtn" onclick="hiddencode('toggle-css')">Hide/Show CSS </button>

<div id="toggle-css" style="display:none">

{% capture text-capture %}
{% raw %}
.terminal {
       background-color: black;
       color: white;
       font-family: "Courier New", Courier, Monospace;
       font-size:large;
   }
.terminalapp {
    background-color: black;
    color: white;
    font-family: "Courier New", Courier, Monospace;
    font-size:large;
    font-style: italic;
  }
.menu {
       background-color: #DCDCDC;
       color: black;
       font-family: "Times New Roman", Times, serif;
       font-size:large;
       border: 1px solid black;
   }
.finder {
        color: black;
        font-family: "Courier New", Courier, Monospace;
        font-size:large;
        font-weight: bold;
   }
.file {
        color: black;
        font-family: "Courier New", Courier, Monospace;
        font-size:large;
        font-weight: bold;
      }
.button {
        background-color: #99ff99;
        color: #006600;
        font-family: "Lucida Grande", Tahoma, Sans-serif;
        border: 1px solid black;
      }
.textbox{
        color: black;
        font-family: "Lucida Grande", Tahoma, Sans-serif;
        border: 1px solid black;
      }
.checkbox{
        background-color: #e6e6ff;
        color: black;
        font-family: "Lucida Grande", Tahoma, Sans-serif;
      }
.tab{
        background-color: #ffffb3;
        color: #392613;
        font-family: "Lucida Grande", Tahoma, Sans-serif;
        border: 1px solid #392613;
    }

.app{
      color: #800000;
      font-family: "Lucida Grande", Verdana, Sans-serif;
    }
.package{
      color: black;
      font-family: $py-font;
    }
.pydef{
      color: black;
      font-family: $py-font;
    }
.module{
      color: black;
      font-family: $py-font;

    }

{% endraw %}
{% endcapture %}

{% include widgets/toggle-code.html  toggle-text=text-capture  %}
</div>

### Common assets and images

As I chose to publish what I do in several thematic blog sites (repositories) I created a solution where I could use a common set of assets (javascript, css, fonts and other resources) across all sites. As almost every Karttur page contains a map (image) at the top, I also changed the YAML for images, created a separate .yml file for images <span class='file>\_data/images.yml</span>,
and assembled all images to the url site that also contains the other common resources. The changes needed to create a separate common resource site is outlined in [another post](../../setup-blog/common-assets/).

### Front matter (\_config.yml and YAML)

The changes above affect the site <span class='file'>\_config.yml</span> and the YAML front matter of all posts. In <span class='file'>\_config.yml</span> the url to the common resources url holding javascript, css, fonts and images must be added.
´´´
commonurl: https://karttur.github.io/common
´´´

The YAML front matter of posts (whether of class post, page or article) also changes compared to the original So Simple theme YAML. The YAML for this page looks like this:

```
---
layout: post
title: Jekyll theme customization
previousurl: null
nexturl null
excerpt: "Costumizing a Jekyll theme"
modified: "2018-01-14 14:33"
categories: blog
tags:
  - Jekyll theme customization
  - So Simple Theme
  - YAML edit
  - assets
image: ts-upsl-rntwi_RNTWI_id_2001-2016_AS
date: "2018-01-14 13:23"
comments: true
share: true
---
```
