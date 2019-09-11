---
layout: post
title: Setup Jekyll theme blog
previousurl: null
nexturl: install-imagemagick
excerpt: "Set up a blog repository with a Jekyll theme, edit using Atom, and publish at GitHub.com"
modified: "2019-09-09 14:33"
categories: "blog"
tags:
  - Blog setup
  - Jekyll
  - So Simple Theme
  - GitHub
image: rntwi-delta_RNTWI_java_2001-2016_mk-z-ts-model
date: "2017-12-30 14:33"
comments: true
share: true
---

**Contents**
	\- [Introduction](#introduction)
	\- [Create a New repository](#create-a-new-repository)
		\- [Select a theme](#select-a-theme)
		\- [Read the theme blog](#read-the-theme-blog)
	\- [Edit the contents of your repository](#edit-the-contents-of-your-repository)
		\- [Create a new post](#create-a-new-post)
		\- [Publish post](#publish-post)
	\- [Resources](#resources)

## Introduction

Looking for a Jekyll template theme to use for writing my blogs, I started by looking at the Jekyll officially [supported themes](https://pages.github.com/themes/), but as I wanted a theme that could handle Tex/LaTex/MathML codes for math, I ended up searching [here](https://jekyllthemes.io) and [here](http://jekyllthemes.org). In the end I decided to use the [So Simple theme](https://github.com/mmistakes/so-simple-theme) by Michael Rose. When writing this in December 2019 the So Simple theme was at version 2. When attempting to [update  Jekyll](../update-gems) and So Simple, I discovered that version 3 of the So Simple theme was differently constructed. **The installation and customisation of the So Simple theme as described in this blog is thus outdated.**

In this blog I describe how I set up Jekyll with the So Simple theme. If you need to set up Jekyll and a text editor for working with your blogs, check [this blog](https://karttur.github.io/setup-blog/). If you want to understand a bit more about the code structure of Jekyll and markdown files, and publish a blog on GitHub, you could read [this blog](https://karttur.github.io/setup-github/).

## Create a New repository

As I will use a new theme (So Simple) for this blog, I chose to create a new repository. As described in an [earlier blog](https://karttur.github.io/setup-github/), I do it using <span class='app'>GitHub desktop.app</span>. In the main menu of <span class='app'>GitHub desktop.app</span>, select

<span class='menu'>File : New Repository</span>

In the dialog window that opens, enter a <span class='textbox'>Name</span> and a <span class='textbox'>Description</span>, check the box for 'Initialize this repository with a README', and leave the alternative for 'GitIgnore' and 'License' with <span class='textbox'>None</span>. Write the local path manually to create a (non-existing) repository. Click the <span class='button'>Create Repository</span> button, and then click <span class='tab'>Publish repository</span> in top menu/tab. In the window that opens, un-check 'Keep this code private', and click <span class='button'>Publish Repository</span>.

### Select a theme

Select a Jekyll theme you like, either from the list of officially [supported themes](https://pages.github.com/themes/), or from other web pages offering Jekyll themes, like [jekyllthemes.io](https://jekyllthemes.io) or [jekyllthemes.org](http://jekyllthemes.org). You can also search directly in GitHub.com. If you are looking for an easy solution, you could use the default Jekyll theme [minima], that I used for creating [my first blog](https://karttur.github.io/setup-blog/).

When downloading your chosen theme, you will be directed to the GitHub repository for that theme. Click the <span class='button'>Clone or Download</span> button, and select either alternative. If you have a local clone of your GitHub.com account it is probably simpler to clone, GitHub.com will ask if you allow opening <span class='app'>GitHub desktop</span> and clone the theme. If you select 'Download ZIP', wait for the download to finish, unzip, and then copy the complete content of the theme folder into the repository folder you just created with <span class='app'>GitHub desktop.app</span>. The <span class='file'>\_config.yml</span> file should be directly under your repository folder. You are copying/moving to the correct folder if your system asks if the <span class='file'>README.md</span> should be overwritten (whether or not to overwrite is your choice, you can anyway edit <span class='file'>README.md</span> later).

Now you should have a repository set up with a Jekyll theme of your choice. Open the <span class='app'>Terminal.app</span> (see this [blog post](https://karttur.github.io/setup-blog/2017/12/21/setup-blog-tools.html#opening-and-understanding-the-terminal) if you are not familiar with the <span class='app'>Terminal</span>). To change directory (cd) to your newly created repository, write <span class='terminal'>$ cd</span> at the prompt, followed by the local path to your repository. Instead of writing the path, you can just drag the path from a <span class='app'>Finder</span> window to the <span class='app'>Terminal</span> prompt, and it will be pasted automatically:

<span class='terminal'>$ cd /local/path/to/repository</span>

List (<span class='terminal'>ls</span>) the content of your repository using the Terminal:

<span class='terminal'>$ ls</span>

The Terminal window will show the files and folders in your repository. They should include <span class='file'>\_config.yml</span>, <span class='file'>README.md</span> and <span class='file'>Gemfile</span>, and more. To install all the dependencies (gems) defined in the <span class='file'>Gemfile</span> start by executing the command:

<span class='terminal'>$ bundle install</span>

You should now be able to use Jekyll to create a local server and view the theme content in your web browser. Write the following command at the <span class='app'>Terminal</span> prompt:

<span class='terminal'>$ bundle exec jekyll serve</span>

### Read the theme blog

Your theme default (instruction) pages should now be up and running as a local server. To read them copy and paste the url returned in the <span class='app'>Terminal</span> window (default is 'http://127.0.0.1:4000'). All themes (that I have tested at least) contain information related both to how to execute Jekyll to serve up the theme, as well as instructions and hints for how to customize the theme.

Dependent on your theme, you might get warnings, or even errors preventing the theme to be served up. Themes created for Jekyll 3.5.0 or earlier, will use 'gems' instead of 'plugins' in <span class='file'>\_config.yml</span>, and dependent on your version of Jekyll and the theme, you might need to edit <span class='file'>\_config.yml</span>. Other syntax changes between different versions of Jekyll and your theme might cause other problems. To solve these problems, you need to use a text editor. In [another blog](https://karttur.github.io/setup-blog/), I introduced <span class='app'>Atom</span>.

## Edit the contents of your repository

Start <span class='app'>Atom</span> (or another text editor if you prefer), and go via the menu:

<span class='menu'>File : New window</span>

The <span class='tab'>New Window</span> will open with some default tabs, including the <span class='tab'>Welcome Guide</span>. Click it and select 'Open a Project', and then click the button that also says <span class='tab'>Open a project</span>. Navigate to the folder containing the repository you created above, and select the repository folder to be your Project.

In the Atom <span class='tab'>Project</span> pane that opens you should see all the files that you put in the repository when you copied the Jekyll theme that you downloaded. If you got a warning saying 'gems' configuration option has been renamed to 'plugins', click on <span class='file'>\_config.yml</span> to open it, and replace 'gems' with 'plugins'.

### Create a new post

<span class='app'>Atom</span> is advertised as ‘A hackable text editor’, and there are several packages available for <span class='app'>Atom</span> that support writing blogs. In [another blog](https://karttur.github.io/setup-blog/) I describe how to install packages. The key package for writing posts using markdown is <span class='menu'>Markdown Writer</span>.

By default, Jekyll expects posts to be located in the repository sub-folder <span class='file'>\_posts</span>, with each post in a separate markdown file. The name of the file **must** start with the date in the form <span class= 'file'>YYYY-MM-DD-</span>. followed by any identification. The full file name of a post thus becomes <span class= 'file'>YYYY-MM-DD-'your-identifier'.markdown</span>

When working on a new post, you do not want it to appear in the online blog, and you should thus not put it in <span class='file'>\_posts</span> folder until it is ready. By default Jekyll ignores any files in the sub-folder <span class='file'>\_drafts</span>. When writing a new post you should hence have your manuscript in the <span class='file'>\_drafts</span> folder.

To start a new post using <span class='app'>Atom</span>, go via the main menu:

<span class='menu'>Packages :  Markdown Writer : File : Add New Draft</span>

In the <span class='tab'>Add New Draft</span> window that opens, the folder <span class='file'>\_drafts</span> is already set, and you only need to give a <span class='textbox'>Title</span>. The title should only be the content identifier, do **not** write the <span class= 'file'>YYYY-MM-DD-</span>, that will be automatically added when using <span class='menu'>Markdown Writer</span> for publishing your blog.

Atom will generate a markdown file with some basic YAML front matter, but it will probably not be adjusted to your Theme. You need to check in the theme documentation and edit the YAML.

When your are finished with the blog, use <span class='menu'>Markdown Writer</span> for publishing your post:

### Publish post

<span class='menu'>Packages : Markdown Writer : File : Publish Draft</span>

This will transfer your file to the <span class= 'file'>\_posts</span> folder, and at the same time add the required prefix <span class= 'file'>YYYY-MM-DD-</span>.

If your server is up and running in "--watch" mode, you should be able to see your new post in your web browser. If not, you need to stop <span class='terminal'>ctrl-x</span> and restart the Jekyll server:

<span class='terminal'>$ jekyll serve --watch</span>

Note that your theme might need another Jekyll command for serving up, if <span class='terminal'>jekyll serve --watch</span> does not work, look at the theme's documentation.

When your are ready with the new post, you need to upload it. To use GitHub.com for publishing online is described in an [earlier blog](https://karttur.github.io/setup-github/)

## Resources

[GitHub supported themes]([supported themes](https://pages.github.com/themes/))

[Set up blog tools: Jekyll and Atom](https://karttur.github.io/setup-blog/) another blog by me

[Setup GitHub pages](https://karttur.github.io/setup-github/) another blog by me
