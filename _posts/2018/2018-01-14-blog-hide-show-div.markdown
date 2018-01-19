---
layout: post
title: Toggle hide/show code
previousurl: null
nexturl: null

modified: 2018-01-14T18:17:25.000Z
categories: "blog"
excerpt: Hide/show div element with code in Jekyll using liquid and javascript.
tags:
  - Jekyll
  - liquid
  - hide/show element
  - CSS
  - javascript
image: ols-sl-twi-percent_MCD43A4_java_2001-2016_A
date: '2018-01-14 19:54'
comments: true
share: true
---
<script src="https://karttur.github.io/common/assets/js/karttur/togglediv.js"></script>
**Contents**
	\- [Introduction](#introduction)
	\- [Javascript solution](#javascript-solution)
		\- [Javascript + liquid + CSS solution](#javascript-liquid-css-solution)

## Introduction

Looking for a way to toggle between hiding/showing chunks of code using Jekyll and markdown, I came across a [bootstrap example](http://tomnorian.com/toggle-code-display-jekyll.html). But I could not find any example using markdown and ordinary CSS. Using the standard javascript solution does not serve up the <div> defining the way the code should be display properly. I solved by using a combination of Jekyll liquids and a predefined <div>.

## Javascript solution

The normal javascript script solution is straight forward.

```
function HideShowElement() {
    var x = document.getElementById("HideShow");
    if (x.style.display === "none") {
        x.style.display = "block";
    } else {
        x.style.display = "none";
    }
}
```
And then a link to the javascript (if not embedded in the html head) and a sender (button) in the body of the html file that invokes the javascript.

```
<script src="url/link/to/togglediv.js"></script>

<button onclick="HideShowElement()">Hide/Show</button>

<div id="HideShow" style="display:none">
  for x in range(10):
    print x*10
    ...

    ...
</div>
```

When used straight the javascript solution does not work with markdown; it collapses the text to one paragraphs, and the code meaning is lost.

### Javascript + liquid + CSS solution

To bypass using only javascript I simply put a liquid inside the element (<div id="HideShow">).

```
<button onclick="HideShowElement()">Hide/Show</button>

<div id="HideShow" style="display:none">

{\% capture text-capture \%}
  "YourCode"
</div>

{\% endcapture \%}

{\% include widgets/toggle-code.html  toggle-text=text-capture  \%}
</div>
```

And then include a very simple widget defining the code block style (<span class='file'>\_includes/widgets/toggle-code.html</span>) in the same way as other code blocks
are defined in the Jekyll Theme. In my theme (So Simple) the code block style is defined using the <div> classes "highlighter-rouge" and "highlight". I just copied that to the widet. I also had to remove the paragraph elements (<p> and </p>) that are otherwise inserted in the <span class='file'>toggle-code.html</span>. And then I can just put a button for toggling hide/show:

<button id= "toggle-codelbtn" onclick="hiddencode('toggle-code')">Hide/Show <span class='file'>toggle-code.html</span> </button>

<div id="toggle-code" style="display:none">

{% capture text-capture %}
{% raw %}
\<div class=\"highlighter-rouge\"><pre class=\"highlight\"\>\<code\>
  {{include.toggle-text | markdownify  | remove: \'<p>\' | remove: \'</p>\' }}
\</code\>\</pre\>
\</div\>
{% endraw %}
{% endcapture %}

{% include widgets/toggle-code2.html  toggle-text=text-capture  %}
</div>
