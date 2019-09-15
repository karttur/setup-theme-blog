---
layout: post
title: Update Jekyll gems
modified: 2019-09-03 11:33
previousurl: null
nexturl: null
categories: "blog"
excerpt: Update Jekyll and a trial to upgrade the theme So Simple
tags:
  - Jekyll
  - gems
  - update
  - So Simple Theme
image: avg-rntwi_RNTWI_java_2001-2016_AS
date: 2019-09-03 11:33
comments: true
share: true
---

## Introduction

At some point in time you will need to upgrade at least your version of Jekyll, and probably also the Jekyll Theme. This post describes how to upgrade Jekyll on Mac OSX installed under a Ruby version that in turn was installed using the Ruby Version Manager (RVM). Redoing the installation from scratch today (September 2019) I would probably have gone for a Homebrew and [rbenv](https://github.com/rbenv/rbenv) ]installation instead.

My intention was to also update the Jekyll Theme that I chose almost 2 years ago, [So Simple](https://github.com/mmistakes/so-simple-theme). The changes between versions 2 and 3 of the So Simple theme, however, break all the customisations I have created to be able to host my diverse pages. I thus decided against updating the theme at this point.

## Check and upgrade RVM version and PATH

If on Mac OSX, start by checking and upgrading the [Ruby Version Manager](https://rvm.io). To check your present RVM version, open a <span class='app'>Terminal</span> and type at the prompt:

<span class='terminal'>$ rvm version</span>

If the response is OK, you can self-upgrade RVM as outlined on the [rvm official upgrade page](https://rvm.io/rvm/upgrading):

<span class='terminal'>$ rvm get stable</span>

If <span class='terminal'>$ rvm version</span> returns an error, like for instance <span class='terminal'>PATH is not properly set up</span>, read the feedback at the prompt and follow the steps. The message <span class='terminal'>PATH is not properly set up</span>, can be solved by making sure that the  <span class='terminal'>PATH</span> to the RVM binary is the very last path given in the .dot-file: <span class='file'>.bash_profile</span> or <span class='file'>.profile</span>. If both these .dot-files exists (both are in your home directory <span class='terminal'>~</span>), you need to make sure that the path to RVM (<span class='terminal'>export PATH="$PATH:$HOME/.rvm/bin"</span>) is the very last of the two. You can edit both <span class='file'>.bash_profile</span> and <span class='file'>.profile</span> using the command-line editor <span class='terminalapp'>pico</span>.

<span class='terminal'>$ pico .profile</span>

Retry checking the RVM version, and if the version is correct, go ahead and update as outlined above. If not try the forced update:

<span class='terminal'>$ rvm get stable --auto-dotfiles</span>

To invoke the installation close and open the <span class='app'>Terminal</span> or run the command <span class='terminal'>$ rvm reload</span>

### Upgrade Ruby under RVM

Before upgrading Ruby itself, note that your existing gems might become outdates and non-responsive. At some time, at least for security reason, you will have to replace outdated gems. There are several internet pages discussing how to go about updating Ruby and your gems, for instance [this page on stack overflow](https://stackoverflow.com/questions/4574318/how-do-i-upgrade-my-ruby-1-9-2-p0-to-the-latest-patch-level-using-rvm).

To check out which versions of Ruby your (upgraded) installation of RVM hosts, open a <span class='app'>Terminal</span> and type at the prompt:

<span class='terminal'>$ rvm list known</span>

Under the heading <span class='terminal'># MRI Rubies</span> the available versions are listed. To install one of the listed versions, type:

<span class='terminal'>$ rvm install X.Y.Z</span>

where X.Y.Z is one the listed versions. At time of writing this I tried installed 2.0.0. It took a very long time, also including upgrading Homebrew and pouring a multitude of new apps and updates while reporting caveats en masse. I understand very little of what happened.

Then I reran the same command (<span class='terminal'>$ rvm install 2.0.0</span>), and this time it worked without any error messages.

```
$ rvm install 2.0.0
Searching for binary rubies, this might take some time.
No binary rubies available for: osx/10.13/x86_64/ruby-2.0.0-p648.
Continuing with compilation. Please read 'rvm help mount' to get more information on binary rubies.
Checking requirements for osx.
[..]
Install of ruby-2.0.0-p648 - #complete
WARNING: Please be aware that you just installed a ruby that is no longer maintained (2017-04-01), for a list of maintained rubies visit:

    http://bugs.ruby-lang.org/projects/ruby/wiki/ReleaseEngineering

Please consider upgrading to ruby-2.6.3 which will have all of the latest security patches
```

As the version I installed was outdated and now longer supported, I again listed the available versions:

<span class='terminal'>$ rvm list known</span>

and sure enough ruby-2.6.3 was the latest. Thus:

<span class='terminal'>$ rvm install 2.6.3</span>

This installation went OK, finishing with the message that Ruby was built without documentation, to build it run:

<span class='terminal'>$ rvm docs generate-ri</span>

To check your installation (for details see the [Jekyll instalaltion instructions](https://jekyllrb.com/docs/installation/)):

<span class='terminal'>$ ruby -v</span>

Check your gem version:

<span class='terminal'>$ gem -v</span>

Check your gem environment:

<span class='terminal'>$ gem env</span>

### Upgrade jekyll

You might need to exit and restart you <span class='terminal'>Terminal</span> session at this time. Then check your Jekyll version:

<span class='terminal'>$ jekyll -v</span>

Following the steps above in September 2019 I ended up with Jekyll version 3.8.6. But version 4 was released in August 2019. As this is the first release of version 4 I opted for not updating at this time. If you need to upgrade to jekyll 4.x.x the [offical Jekyll Updgrage page](https://jekyllrb.com/docs/upgrading/) have instructions for migrating between major Jekyll versions.

#### Upgrade repository gems

Before doing the upgrade described most of my repositories were at jekyll version 3.6.2, and I got some security warnings. To upgrade a reposiroy, simply open the <span class='file'>Gemfile</span> and alter the line:

```
gem "jekyll", "3.6.2"
```
to
```
gem "jekyll", "3.8.6"
```

Close and save the <span class='file'>Gemfile</span> and run the command:

<span class='terminal'>bundle update</span>

or

<span class='terminal'>bundle update jekyll</span>

### Upgrade Jekyll Theme

After having upgraded Jekyll and its dependencies it was my intention to also upgrade the Jekyll theme ([So Simple](https://github.com/mmistakes/so-simple-theme)) I use for my pages. The change from version 2 to version 3, however, turned out to be a major overhaul. To the extent that I would have to create a completely new set of customisations. For me, upgrading the SO Simle theme would mean the same effort as migrating to a completely new theme. At this time I did not have the time for this.
