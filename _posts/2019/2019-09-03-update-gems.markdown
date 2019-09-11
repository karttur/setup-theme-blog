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


## introduction

At some point in time you will need to upgrade at least your version of Jekyll, and probably also the Jekyll Theme. This post describes how to upgrade Jekyll installed under a Ruby version that in turn was installed using the Ruby Version Manager (RVM). Redoing the installation from scratch today (September 2019) I would probably have gone for a Homebrew installation instead.

My intention was to also update the Jekyll Theme that I chose almost 2 years ago, [So Simple](https://github.com/mmistakes/so-simple-theme). The changes between versions 2 and 3 of the So Simple theme, however, break all the customisations I have created to be able to host my diverse information. I decided against updating the theme at this point.

## Check and upgrade RVM version and PATH

If on Mac OSX, start by checking and upgrading the [Ruby Version Manager](https://rvm.io). To check your present RVM version, open a <span class='app'>Terminal</span> and type at the prompt:

<span class='terminal'>$ rvm version</span>

If the response is OK, you can self-upgrade RVM as outlined on the [rvm official upgade page](https://rvm.io/rvm/upgrading):

<span class='terminal'>$ rvm get stable</span>

If <span class='terminal'>$ rvm version</span> returns an error, like for instance <span class='terminal'>PATH is not properly set up</span>, read the feedback at the prompt and follow the steps. The message <span class='terminal'>PATH is not properly set up</span>, can be solved by making sure that the  <span class='terminal'>PATH</span> to the RVM binary is the very last path given in the .dot-file: <span class='file'>.bash_profile</span> or <span class='file'>.profile</span>. If both these .dot-files exists (they are in in your ome directory <span class='terminal'>h</span>), you need to make sure that the path to RVM (<span class='terminal'>export PATH="$PATH:$HOME/.rvm/bin"</span>) is the very last of the two. You can edit both <span class='file'>.bash_profile</span> and <span class='file'>.profile</span> using the command-line editor <span class='terminalapp'>pico</span>.


<span class='terminal'>$ pico .profile</span>

Retry checking the RVM version, and if the version is correct, go ahead and update as outlined above. If not try the forced update:

<span class='terminal'>$ rvm get stable --auto-dotfiles</span>

To invoke the installation close and open the <span class='app'>Terminal</span> or run the command <span class='terminal'>$ rvm reload</span>

### Upgrade Ruby under RVM

Before upgrading Ruby itself, note that your existing gems might become outdates and non-responsive. At some time, at least for security reason, you will have to replace outdated gems. There are several internet pages discussing how to go about updating Ruby and your gems, for instance [this page on stack overflow](https://stackoverflow.com/questions/4574318/how-do-i-upgrade-my-ruby-1-9-2-p0-to-the-latest-patch-level-using-rvm).

To check out which versions of Ruby your (upgraded) installation of RVM hosts, open a class='app'>Terminal</span> and type at the prompt:

<span class='terminal'>$ rvm list known</span>

Under the heading <span class='terminal'># MRI Rubies</span> the available versions are listed. To install one of the listed versions, type:

<span class='terminal'>$ rvm install X.Y.Z</span>

where X.Y.Z is one the listed versions (at time of writing this I tried installed 2.0.0). It took a very long time, also including upgrading Homebrew and pouring a multitude of new apps and updates while reporting caveats en masse. I understand very little of what happened.

Then I reran the same command (<span class='terminal'>$ rvm install 2.0.0</span>), and this time it worked without any error messages.

```
$ rvm install 2.0.0
Searching for binary rubies, this might take some time.
No binary rubies available for: osx/10.13/x86_64/ruby-2.0.0-p648.
Continuing with compilation. Please read 'rvm help mount' to get more information on binary rubies.
Checking requirements for osx.
...
...
Install of ruby-2.0.0-p648 - #complete
WARNING: Please be aware that you just installed a ruby that is no longer maintained (2017-04-01), for a list of maintained rubies visit:

    http://bugs.ruby-lang.org/projects/ruby/wiki/ReleaseEngineering

Please consider upgrading to ruby-2.6.3 which will have all of the latest security patches
```

As the version I installed was outdated and now longer supported, I again listed the available versions:

<span class='terminal'>$ rvm list known</span>

and sure enough ruby-2.6.3 was the latest. Thus:

<span class='terminal'>$ rvm install 2.6.3</span>

```
Searching for binary rubies, this might take some time.
No binary rubies available for: osx/10.13/x86_64/ruby-2.6.3.
Continuing with compilation. Please read 'rvm help mount' to get more information on binary rubies.
Checking requirements for osx.
```

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

At time of writing this I had 3.6.2 installed, and the Jekyll Theme that I use for my web-pages ([SoSimple(https://github.com/mmistakes/so-simple-theme)]) requires 3.5 or higher. If you need to upgrade to jekyll 4.x.x the [offical Jekyll Updgrage page](https://jekyllrb.com/docs/upgrading/) have instructions for migrating between major Jekyll versions.

### Upgrading Jekyll Theme

At this stage you should have a version of Jekyll that allows you to upgrade your Jekyll Theme. In my case SoSimple. I have also made customisations to my theme, and need to consider that. If you are hosting your Jekyll built pages on [GitHub](https://github.com), you can now use all Jekyll Themes stored in GitHub repositories as gems (I think). I am not sure about this, and whether or not you can include customizations if you use the repository as your Jeyll gem (or however it is to be expressed). You really have to be a nerd to understand this.

#### Testing with a new repo

Here is how I tried it out, folliwng the SoSimple tutorial online.

##### 1. Download latest version SoSimple

##### 2. Edit Gemfile

I added the last two lines (the first two were already in place):

```
source "https://rubygems.org"
gemspec
# use local theme gem for testing
gem "jekyll-theme-so-simple", path: "../"
```

##### 3. Edit \_config.yml

The instructions by Mr Rose (the createor of SoSimple) then states that the line
```
theme: jekyll-theme-so-simple
```
should be added to the site file <span class='file'>\_config.yml</span>. But in my downloaded copy it was already there.

##### 4. Run bundler

In the <span class='app'>Terminal</span> Change directory <span class='terminal'>cd</span> to the directory where you saved the downloaded copy of your selected theme.

<span class='terminal'>$ cd path/to/theme_master_folder</span>

Then run the <span class='terminalapp'>bundle install</span>

<span class='terminal'>$ bundle install</span>

And then it crashes, repporting that you can not hae two versions of the same theme linked as there is then a conflict.

```
Your Gemfile lists the gem jekyll-theme-so-simple (>= 0) more than once.
You should probably keep only one of them.
While it's not a problem now, it could cause errors if you change the version of one of them later.

[!] There was an error parsing `Gemfile`: You cannot specify the same gem twice coming from different sources.
You specified that jekyll-theme-so-simple (>= 0) should come from source at `.` and
. Bundler cannot continue.

 #  from /Users/thomasgumbricht/GitHub/sosimple_template_20190911/Gemfile:4
 #  -------------------------------------------
 #  # use local theme gem for testing
 >  gem "jekyll-theme-so-simple"
 #  -------------------------------------------
```

I thus removed the lines I added in the <span class='file'>Gemfile</span>, and then it worked out, with the terminal reporting:

```
Bundle complete! 3 Gemfile dependencies, 37 gems now installed.
```

#### Build from scratch

Change directory <span class='terminal'>cd</span> to the parent folder of your repos (or projects). Then create a new Jekyll site by typing:

<span class='terminal'>$ jekyll new "mytestsite"</span>


##### Edit Gemfile

Open the Gemfile (e.g in Atom) and make these edits:

replace
```
gem "minima", "~> 2.0"
```
with
```
gem "jekyll-theme-so-simple"
```

and
```
gem "jekyll", "~> 3.8.6"
```
with
```
gem "github-pages", group: :jekyll_plugins
```

The edited Gemfile then looks like this:
```
source "https://rubygems.org"

# Hello! This is where you manage which Jekyll version is used to run.
# When you want to use a different version, change it below, save the
# file and run `bundle install`. Run Jekyll with `bundle exec`, like so:
#
#     bundle exec jekyll serve
#
# This will help ensure the proper Jekyll version is running.
# Happy Jekylling!
#gem "jekyll", "~> 3.8.6"

# This is the default theme for new Jekyll sites. You may change this to anything you like.
gem "minima", "~> 2.0"
gem "jekyll-theme-so-simple", path: "../"

# If you want to use GitHub Pages, remove the "gem "jekyll"" above and
# uncomment the line below. To upgrade, run `bundle update github-pages`.
gem "github-pages", group: :jekyll_plugins

# If you have any plugins, put them here!
group :jekyll_plugins do
  gem "jekyll-feed", "~> 0.6"
end

# Windows does not include zoneinfo files, so bundle the tzinfo-data gem
# and associated library.
install_if -> { RUBY_PLATFORM =~ %r!mingw|mswin|java! } do
  gem "tzinfo", "~> 1.2"
  gem "tzinfo-data"
end

# Performance-booster for watching directories on Windows
gem "wdm", "~> 0.1.0", :install_if => Gem.win_platform?

```

##### Edit \_config.yml

replace
```
theme: minima
```
with
```
theme: jekyll-theme-so-simple
```

Then you can also change the following tags:

- title
- email
- description
- baseurl
- url
- twitter_username
- github_username


### Build local site

Build the site and make it available on a local server

<span class='terminal'>$ bundle exec jekyll serve</span>

Then change directory <span class='terminal'>$ cd "mytestsite"</span> into your testsite.

## Resources

[Karttur´s "common" repository](#)

[Jekyll installation (incl Ruby)][https://jekyllrb.com/docs/installation/]
