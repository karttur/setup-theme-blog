---
layout: post
title: Github.io front page
modified: 2019-09-09 10:33
previousurl: null
nexturl: null
categories: "blog"
excerpt: Create the page for github.io/user
tags:
  - Jekyll
  - gems
  - update
  - So Simple Theme
image: avg-rntwi_RNTWI_java_2001-2016_AS
figure1: github-account_karttur_01_menu-new-repo
date: 2019-09-09 10:33
comments: true
share: true
---
**Contents**
\- [Introduction](#introduction)
\- [Prerequisits](#prerequisits)
\- [Create the front page repository](#create-the-front-page-repository)
\- [Keep the code in the Master branch](#keep-the-code-in-the-master-branch)
\- [Clone your repository](#clone-your-repository)

## Introduction

All the gh-pages that you publish on github (http://user.github.io) end up in sub-folders named after the repository (e.g. http://user.github.io/"repositiry"). This post explains how to create a front page at http://user.github.io.

## Prerequisits

If you just want to build a web page from scratch, all you need is a [github.com]() account. If you want to manage your domain web page from your local machne you also need <span class='app'>GitHub Desktop</span>.

## Create the front page repository

Log in to your GitHub account and click the plus sign in the upper right corner, from the drop down menu select <span class='button'>New repository</span>. You can also to directly to [https://github.com/new](https://github.com/new) and you will get to page Create a new repository.

<figure>
<img src="{{ site.commonurl }}/images/{{ site.data.images[page.figure1].file }}">
<figcaption> {{ site.data.images[page.figure1].caption }} </figcaption>
</figure>

To create the front page for your gitub.io domain account, give your new repository the name <span class='menu'>_user_.github.io</span>, replacing _user_ with your GitHub username (i.e. karttur in my case).

<span class='menu'>karttur.github.io</span>

Add a <span class='textbox'>Description</span> and also check <span class='checkbox'>Initialise this repository with a README</span>. Then click <span class='button'>Create Repository</span>

## Keep the code in the Master branch

When using GitHub for building/deploying html pages normally the special (GitHub self-aware) branch called gh-pages must be used. Not so for the domain front page, here the html (markdown) and associated resources (style sheets, scripts etc) should be associated with the Master branch.

If you want to build a customised front page directly in github.com, you can for instance find instructions at [Creating and Hosting a Personal Site on GitHub](http://jmcglone.com/guides/github-pages/) by Jonathan McGlone. As my pages are built on a customised version of a Jekyll theme, I prefer to clone the repository to a local computer to develop the domain front page on my local machine.

### Clone your repository

Clone the repository to you local machine, either using <span class='app'>GitHub Desktop</span> or the command-line tool <span class='terminal'>git</span>.
