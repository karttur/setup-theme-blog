---
layout: post
title: Customisation summary
modified: 2019-10-09 10:33
previousurl: null
nexturl: null
categories: "blog"
excerpt: A summary of my costutmisations in code
tags:
  - Jekyll
  - customization
  - So Simple Theme
image: avg-rntwi_RNTWI_java_2001-2016_AS
figure1: github-account_karttur_01_menu-new-repo
date: 2019-10-09 10:33
comments: true
share: true
---
**Contents**

## introduction

I wrote this summary as a preparation for changing to the rewokree version of the Jekyll theme [So Simple](https://github.com/mmistakes/so-simple-theme) by Michael Rose. The new version, however, is so different that I did not go through with changing all my customizations.

## Customisation

### edited and extended scss

#### \_animations.scss

No changes.

#### \_archive.scss

Changed from "width: 80%" to "width: 70%" as shown below.

```
/* post excerpt */
.excerpt {
  display: block;
  float: none;
  @include font-size(14, no, 16);

  @include media($medium) {
    width: 70%; #Changed from 80%
  }

  @include media($large) {
    width: 60%;
  }
```

#### \_base.scss

No changes.

#### \_buttons.scss

No changes.

#### \_footer.scss

Added .foot-title and .foot-description

```
.foot-title {
  margin-bottom: 0;
  color: black;
  font-style: normal;
}

/*
   Site description text
   ========================================================================== */

.foot-description {
  margin-top: 0;
  font-family: $alt-font;
  @include font-size(20);
  font-weight: 400;
  font-style: italic;
  color: black;
}
```

#### \_footnotes.scss

No changes.

#### \_forms.scss

No changes.

#### \_grid-settings

No changes.

#### \_helpers

No changes.

#### \_masthead

No changes.

#### \_mixins

No changes.

#### \_navigation

No changes.

#### \_notices

No changes.

#### \_page.scss

The page layout is changed to have somewhat smaller font sizes and less of margins.

```
/* ==========================================================================
   Page/post layout and styling
   ========================================================================== */

/*
   Main content
   ========================================================================== */

#main {
	@include clearfix;
}

.entry,
.hentry {
	@include clearfix;
	border-bottom: 1px solid lighten($black,80);
	border-bottom: 1px solid rgba($black,.10);
}

/* feature image */

.entry-feature-image {
	margin: -75px 0 0 0;
	/*margin-top: -75px; /* move up to be overlapped by site logo */
	width: 100%;

	@include media($medium) {
		margin-top: -75px; /* move up to be overlapped by site logo */
	}

	@include media($large) {
		margin-top: -230px; /* move up further to be overlapped by site logo */
	}
}

/* page header */

.entry-header {
	@include fill-parent;
}

/* tag listing in page header */

.entry-tags {
	margin: 1em 0 0;
  padding: 0;
	text-transform: uppercase;
	@include font-size(16);
	font-weight: 600;

  a {
    color: $text-color;
    padding: 0 5px;
  }

  li {
    display: inline-block;
    margin-bottom: 0;

    &:before {
      content: "\2022";
    }

    &:first-child {

      &:before {
        content: "";
      }

      a {
        padding-left: 0;
      }
    }
  }
}

/* page title */

span + .entry-title {
	margin-top: 0;
}

.entry-title {
	font-family: $alt-font;
	font-style: italic;
	@include font-size(32,yes,32);
	font-weight: 400;
	line-height: 1;
	letter-spacing: -3px;

	a {
		color: $black;
		text-decoration: underline;
	}

	@include media($medium) {
		@include font-size(44,yes,44);
	}

	@include media($large) {
		@include font-size(60,yes,64);
	}
}

/* page/post wrapper */

.entry-wrapper {
	@include outer-container;
	margin-top: 0;
	margin-bottom: 1em;
	padding-right: $gutter;
	padding-left: $gutter;
}

/* page/post meta content (date, author, etc) */

.entry-meta {
	@include span-columns(12);
	text-transform: uppercase;
	@include font-size(14);

	a {
		color: $text-color;
	}

	@include media($large) {
		@include span-columns(2.5);
	}

	& > span {
		padding: 0 10px 5px 0;
		display: inline-block;

		@include media($large) {
			display: block;
			padding: 8px 0;
			border-bottom: 1px solid lighten($black,80);
			border-bottom: 1px solid rgba($black,.10);
		}
	}
}

/* author avatar (circular) */

.bio-photo {
	display: none;

	@include media($large) {
		display: block;
		width: 150px;
		height: 150px;
		margin-bottom: 10px;
		@include rounded(150px);
		@include clearfix;
	}
}


/* feature image credit */

.image-credit {
  margin: 0 auto;
  max-width: 440px;
  padding-top: 5px;
  padding-right: 20px;
  padding-left: 20px;
  text-align: right;
  @include font-size(12, no);
  line-height: 1.3;
  color: lighten($text-color, 30);
  @include clearfix();

  @include media($medium) {
    max-width: 760px;
    padding-right: 60px;
    padding-left: 60px;
    @include font-size(14, no);
  }

  @include media($large) {
    max-width: 960px;
  }

  a {
    color: lighten($text-color, 30);
  }
}

/* main content block */

.entry-content {
	@include span-columns(12);

	p:first-child {
		margin-top: 0;
	}

	@include media($large) {
		@include span-columns(9.5);
	}

	/* nice link underlines */
  p > a,
	li > a {
		border-bottom: 1px dotted lighten($link-color, 50);

		&:hover {
			border-bottom-style: solid;
		}
	}
}

/*
   Disqus
   ========================================================================== */

#disqus_thread {
	margin-top: 2em;
}

/*
   Pagination
   ========================================================================== */

.pagination {
	margin-top: 2em;
	text-align: center;
}

/*
   Overrides
   ========================================================================== */

/* adjust width for lack of meta/author column */

#home,
#page {

	.entry-wrapper {
		max-width: em(760);
	}

	.entry-title {
		text-align: center;
		max-width: 100%;
	}

	.entry-content {
		@include span-columns(12);
	}
}

/*
   Kramdown generated table of contents
   ========================================================================== */

#markdown-toc {
	font-family: $alt-font;
	margin-top: $gutter;
	margin-bottom: $gutter;
	padding-left: 0;
	border: 1px solid $border-color;
	border-radius: $border-radius;

  ul {
  	list-style-type: none;
  	padding-left: 0;
  }

  li {
    @include font-size(16,no,18);
    border-bottom: 1px solid $border-color;
    list-style-type: none;
  }

  h6 {
    margin: 0;
    padding: (.25 * $gutter) (.5 * $gutter);
    background: $table-stripe-color;
  }

  a {
    display: block;
    padding: (.25 * $gutter) (.5 * $gutter);
    border-left: 2px solid transparent;
    border-bottom: 0 solid transparent;

    &:hover,
    &:focus {
      background: lighten($border-color,5);
    }
  }
}

/*
   Tables
   ========================================================================== */

/** For nicer looking tables apply the .table class
 *  Example:
 *  <table class="table">
 *    <tr>
 *      <td>cell1</td>
 *      <td>cell2</td>
 *      <td>cell3</td>
 *    </tr>
 *  </table>
*/

.table {
	border-collapse: collapse;
	margin: ((0px + $doc-line-height) / 2) 0;
	margin: ((0rem + ($doc-line-height / $doc-font-size)) / 2) 0;
	width: 100%;

	tbody {

		tr:hover > td, tr:hover > th {
			background-color: $table-hover-color;
		}
	}

	thead {

		tr:first-child td {
			border-bottom: 2px solid $table-border-color;
		}
	}

	th {
		padding: (0px + $doc-line-height) / 2;
		padding: (0rem + ($doc-line-height / $doc-font-size)) / 2;
		font-family: $alt-font;
		font-weight: bold;
		text-align: left;
		background-color: $table-header-color;
		border-bottom: 1px solid darken($border-color, 15%);
	}

	td {
		border-bottom: 1px solid $border-color;
		padding: (0px + $doc-line-height) / 2;
		padding: (0rem + ($doc-line-height / $doc-font-size)) / 2;
		@include font-size(18);
	}

	tr, td, th {
		vertical-align: middle;
	}
}
```

#### \_reset

No changes.

#### \_search

No changes.

#### \_syntax.css

The following code is added to \_syntax.css for allowing different layout of the \<span\> elements.
```
/*
   Styles for span classes to identify different object types
   ========================================================================== */
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
```

#### \_variables

Added $py-font

```
// TYPOGRAPHY ================================================
$base-font                : 'source-sans-pro', sans-serif;
$heading-font             : $base-font;
$caption-font             : $base-font;
$code-font                : 'source-code-pro', monospace;
$py-font                  : 'Consolas', monospace;
$alt-font                 : 'volkhov', serif;
```

#### \_wells

No changes.

### \_includes

#### ad-footer.html

No changes.

#### ad-sidebar.html

No changes.

#### browser-upgrade.html

No changes.

#### disqus-comments.html

No changes.

#### feed-footer.html

No changes.

#### footer.html

footer.html edited to include title and description (\<div class wrap \> below) in footer (c.f. the foot-title and foot-description added to \_footer.scss above). Social icons updated regarded linkedin connection and inclusion of ResearchGate.

```
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
```

#### head.html

If problems, consider removing the tags
```
\{\% seo \%\}
```

Replace "url" with "commonurl" for logo.
```
<div class="navigation-wrapper">
	<nav role="navigation" id="site-nav" class="animated drop">
	    <ul>
      {% for link in site.data.navigation %}
		    {% if link.url contains 'http' %}
		        {% assign domain = '' %}
		        {% else %}
		        {% assign domain = site.url %}
		    {% endif %}
		    <li><a href="{{ domain }}{{ link.url }}" {% if link.url contains 'http' %}target="_blank"{% endif %}>{{ link.title }}</a></li>
		  {% endfor %}
	    </ul>
	</nav>
</div><!-- /.navigation-wrapper -->

{% include browser-upgrade.html %}

{% if page.image %}
<header class="masthead">
	{% if site.logo != null %}
		<div class="wrap">
			<a href="{{ site.url }}/" class="site-logo" rel="home" title="{{ site.title }}"><img src="{{ site.commonurl }}/images/{{ site.logo }}" width="200" height="200" alt="{{ site.title }} logo" class="animated fadeInDown"></a>
		</div>
	{% endif %}
</header><!-- /.masthead -->
{% else %}
<header class="masthead">
	<div class="wrap">
      {% if site.logo != null %}
  		<a href="{{ site.url }}/" class="site-logo" rel="home" title="{{ site.title }}"><img src="{{ site.commonurl }}/images/{{ site.logo }}" width="200" height="200" alt="{{ site.title }} logo" class="animated fadeInDown"></a>
      {% endif %}
      <h1 class="site-title animated fadeIn"><a href="{{ site.url }}/">{{ site.title }}</a></h1>
			<h2 class="site-description animated fadeIn" itemprop="description">{{ site.description }}</h2>
	</div>
</header><!-- /.masthead -->{% endif %}

<div class="js-menu-screen menu-screen"></div>
```

#### navigation.html

No changes.

#### open-graph.html

No changes.

#### scripts.html

No changes.

Needs to be fixed regarding common assets and also for the hideshowdiv script connection.

#### social-share.html

No changes.

### widgets

#### toggle-code.html

Added file toggle-code.html.

```
<div class="highlighter-rouge"><pre class="highlight"><code>
  {{include.toggle-text | markdownify  | remove: '<p>' | remove: '</p>' }}
</code></pre>
</div>
```

#### xml resources

under widgets a new folder called xml is used for storing xml code in html formats. The xml to html conversion is done with python package <span class='package'>jekyllize</span>
